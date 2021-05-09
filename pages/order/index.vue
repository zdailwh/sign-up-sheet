<template>
	<view>
		<view v-if="!loading">
			<view v-if="orders.length">
				<view class="orderWrap" v-for="(order,index) in orders" v-key="index">
					<view class="formTitle">
						<view class="line1">您已预约成功！</view>
						<view class="line2">{{order.apply_date}}</view>
						<view class="line2">{{parseInt(order.am_pm) === 1? '上午' : '下午'}}</view>
						<view class="line2">{{order.no}}号</view>
						<view class="line3">预计您的办理时间在{{parseInt(order.am_pm) === 1? '上午' : '下午'}} {{ order.begintime }}，请提前10分钟到场！</view>
					</view>
					<uni-list>
						<uni-list-item title="学生姓名" :rightText="order.name"></uni-list-item>
						<uni-list-item title="性别" :rightText="parseInt(order.sex) === 1? '男' : '女'"></uni-list-item>
						<uni-list-item title="身份证号码" :rightText="order.idc_no"></uni-list-item>
						<uni-list-item title="联系电话" :rightText="order.contract_number"></uni-list-item>
						<uni-list-item title="户口所在地" :rightText="order.province + ' ' + order.city + ' ' + order.area"></uni-list-item>
						<uni-list-item title="家庭住址" :rightText="order.officer_name"></uni-list-item>
						<uni-list-item title="预约时间" :rightText="order.apply_date + ' ' + (parseInt(order.am_pm) === 1? '上午' : '下午')"></uni-list-item>
						<uni-list-item title="排号" :rightText="order.no"></uni-list-item>
					</uni-list>
				</view>
			</view>
			<view v-else class="noneOrder">
				您还没有预约记录！
			</view>
			<view class="notice">
				查看<navigator url="/pages/index/notice" hover-class="none" class="link">《招生简章》</navigator>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			loading: true,
			orders: []
		}
	},
	onShow() {
		if (!uni.getStorageSync('session_key') || !uni.getStorageSync('user_id')) {
			console.log('去登录')
			this.login()			
		} else {
			this.getOrders()
		}
	},
	methods: {
		getOrders() {
			uni.showLoading()
			uni.request({
			    url: 'https://school.jiankangzhuzhang.com/apply/list?user_id=' + uni.getStorageSync('user_id'), 
				method: 'GET',
			    header: {
			        'token': uni.getStorageSync('session_key') //自定义请求头信息
			    },
			    success: (res) => {
			        console.log(res.data)
					this.loading = false
					uni.hideLoading()
					if (res.data.httpCode === 200) {
						this.orders = res.data.data || []
						this.orders.map((item) => {
							if (item.am_pm === 1) {
								var minit = (item.no - 1) * 4
								var begintime = this.minitPlus('8:00', minit)
							} else if (item.am_pm === 2) {
								var minit = (item.no - 1) * 4
								var begintime = this.minitPlus('13:00', minit)
							}
							item.begintime = begintime
						})
					} else if (res.data.message === '提供的信息已经过期，请重新登录') {
						this.login()
					} else {
						uni.showToast({
						    title: res.data.message,
							icon: 'none',
						    duration: 3000
						})
					}
			    },
				fail: (err) => {
					this.loading = false
					uni.hideLoading()
					console.log(err)
					uni.showToast({
					    title: '请求出错，请稍后再试！',
						icon: 'none',
					    duration: 3000
					})
				}
			})
		},
		minitPlus (time, minit) {
			var h = parseInt(time.split(':')[0])
			var m = parseInt(time.split(':')[1])
			var totalM = m + minit
			if (totalM >= 60) {
				h = parseInt(totalM / 60) + h
				m = totalM % 60
			} else {
				m = totalM
			}
			return h + ':' + this.padStart(m)
		},
		padStart(val) {
			if (val < 10) {
				return '0' + val
			} else {
				return val
			}
		},
		//登录
		login() {
			let _this = this
			uni.showLoading({
				title: '登录中...'
			})	 
			// 1.wx获取登录用户code
			uni.login({
				provider: 'weixin',
				success: function(loginRes) {
					let code = loginRes.code
					//将用户登录code传递到后台置换用户SessionKey、OpenId等信息
					uni.request({
						url: 'https://school.jiankangzhuzhang.com/user/login',
						data: {
							code: code,
						},
						method: 'POST',
						header: {
							'content-type': 'application/json'
						},
						success: (res) => {
							//openId、或SessionKdy存储//隐藏loading
							console.log('login response')
							console.log(res)
							uni.setStorageSync('session_key', res.data.data.session_key)
							uni.setStorageSync('user_id', res.data.data.user_id)
							uni.hideLoading()
							_this.getOrders()
						},
						fail: (res) => {
							uni.hideLoading()
							console.log(res.errMsg)
						}
					})
				}
			})
		}
	}
}
</script>

<style>
page {
	background-color: #9fd7e6;
}
.formTitle {
	text-align: center;
	padding: 15rpx;
}
.formTitle .line1 {
	font-size: 40rpx;
	color: #666;
	line-height: 60rpx;
}
.formTitle .line2 {
	font-size: 50rpx;
	font-weight: bold;
	color: #E93B3D;
	line-height: 80rpx;
}
.formTitle .line3 {
	font-size: 30rpx;
	color: #666;
	line-height: 50rpx;
}
.uni-list-item__content-title {
	font-size: 30rpx !important;
}
.uni-list-item__extra-text {
	font-size: 30rpx !important;
}
.orderWrap {
	margin: 30rpx 30rpx 30rpx;
	border-radius: 20rpx;
	background-color: rgba(255,255,255,.7) !important;
}
.orderWrap .uni-list {
	background-color: transparent;
}
.orderWrap .uni-list-item {
	background-color: transparent;
}
.noneOrder {
	font-size: 35rpx;
	text-align: center;
	margin: 50rpx;
}
.notice {
	margin: 50rpx 0;
	text-align: center;
	padding: 0 30rpx;
	font-size: 30rpx;
	color: #333;
	line-height: 2;
}
.notice .link {
	display: inline-block;
	color: #007AFF;
}
</style>
