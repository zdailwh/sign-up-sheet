<template>
	<view>
		<view v-if="!loading">
			<view v-if="orders.length">
				<view class="formTitle">您已成功预约！</view>
				<view class="orderWrap" v-for="(order,index) in orders" v-key="index">
					<uni-list>
						<uni-list-item title="学生姓名" :rightText="order.name"></uni-list-item>
						<uni-list-item title="性别" :rightText="parseInt(order.sex) === 1? '男' : '女'"></uni-list-item>
						<uni-list-item title="身份证号" :rightText="order.idc_no"></uni-list-item>
						<uni-list-item title="联系电话" :rightText="order.contract_number"></uni-list-item>
						<uni-list-item title="户口所在地" :rightText="order.province + ' ' + order.city + ' ' + order.area"></uni-list-item>
						<uni-list-item title="派出所" :rightText="order.officer_name"></uni-list-item>
						<uni-list-item title="预约时间" :rightText="order.apply_date + ' ' + (parseInt(order.am_pm) === 1? '上午' : '下午')"></uni-list-item>
					</uni-list>			
				</view>
			</view>
			<view v-else class="noneOrder">
				您还没有预约记录！
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
	onLoad() {
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
	background-color: #f5f5f5;
}
.formTitle {
	padding-left: 30rpx;
	height: 100rpx;
	line-height: 100rpx;
	font-size: 32rpx;
	color: #000;
}
.uni-list-item__content-title {
	font-size: 30rpx !important;
}
.uni-list-item__extra-text {
	font-size: 30rpx !important;
}
.orderWrap {
	margin-bottom: 30rpx;
}
.noneOrder {
	font-size: 40rpx;
	text-align: center;
	margin: 50rpx;
}
</style>
