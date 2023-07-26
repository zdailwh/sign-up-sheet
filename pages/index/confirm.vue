<template>
	<view class="">
		<uni-forms :value="formData" ref="form" labelWidth="80">
			<view class="formTitle">请确认以下信息准确无误</view>
			<view class="formWrap">
				<uni-forms-item label="学生姓名" name="name" label-align="left">
					<view class="inputView">{{formData.name}}</view>
				</uni-forms-item>
				<uni-forms-item name="sex" label="性别" label-align="left">
					<view class="inputView">{{parseInt(formData.sex) === 1? '男' : '女'}}</view>
				</uni-forms-item>
				<uni-forms-item label="身份证号码" name="idc_no" label-align="left">
					<view class="inputView">{{formData.idc_no}}</view>
				</uni-forms-item>
				<uni-forms-item label="联系电话" name="contract_number" label-align="left">
					<view class="inputView">{{formData.contract_number}}</view>
				</uni-forms-item>
				<uni-forms-item label="户籍所在地" name="household" label-align="left">
					<view class="inputView">{{formData.province + ' ' + formData.city + ' ' + formData.area}}</view>
				</uni-forms-item>
				<uni-forms-item label="家庭住址" name="officer_name" label-align="left">
					<view class="inputView">{{formData.officer_name}}</view>
				</uni-forms-item>
				<uni-forms-item label="报名日期" name="apply_date" label-align="left">
					<view class="inputView">{{formData.apply_date}}</view>
				</uni-forms-item>
				<uni-forms-item label="时间段" name="am_pm" label-align="left">
					<view class="inputView">{{parseInt(formData.am_pm) === 1? '上午' : '下午'}}</view>
				</uni-forms-item>
			</view>
		</uni-forms>
		<view class="btnWrap">
			<button class="btn1" type="primary" @click="submitForm">确认提交</button>
			<navigator url="/pages/index/index" open-type="switchTab" hover-class="none"><button class="btn2" type="primary" plain="true">上一步 </button></navigator>			
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				applyRes: {},
				formData: {}
			}
		},
		onLoad: function (option) {
			uni.showShareMenu()
			this.formData = JSON.parse(option.formData) || {}
		},
		methods: {
			submitForm() {
				// uni.redirectTo({
				// 	url: '/pages/result/success?order=' + JSON.stringify(this.formData)
				// })
				if (this.formData.user_id) {
					uni.showLoading({
						title: '正在提交'
					})
					uni.request({
						url: 'https://houtai.jcawxx.cn/user/apply', 
						method: 'POST',
						data: this.formData,
						header: {
							'token': uni.getStorageSync('session_key') //自定义请求头信息
						},
						success: (res) => {
							uni.hideLoading()
							if (res.data.httpCode === 200 && res.data.message === '预约成功') {
								this.applyRes = res.data.data
								uni.redirectTo({
									url: '/pages/result/success?order=' + JSON.stringify(this.applyRes)
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
							uni.hideLoading()
							console.log(err)
							uni.showToast({
								title: '请求出错，请稍后再试！',
								icon: 'none',
								duration: 3000
							})
						}
					})
				} else {
					uni.showToast({
						title: '无效userid',
						icon: 'none',
						duration: 3000
					})
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
							url: 'https://houtai.jcawxx.cn/user/login',
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
								_this.submitForm()
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
		background-color: #fff;
	}
	.uni-forms-item__label .label-text {
		font-size: 30rpx !important;
	}
	.uni-forms-item__inner {
		padding-bottom: 0 !important;
	}
	.formTitle {
		padding-left: 30rpx;
		height: 100rpx;
		line-height: 100rpx;
		font-size: 32rpx;
		color: #666;
	}
	.formWrap {
		padding: 15px 15px 0 15px;
		/* background-color: rgba(159,215,230,.5); */
		background-color: rgba(0,122,255,.1);
	}
	.btnWrap {
		padding: 15px;
	}
	.btnWrap button.btn1 {
		background-color: #206fb1;
	}
	.btnWrap button.btn1[disabled] {
		background-color: #206fb1;
	}
	.btnWrap button.btn1:hover {
		background-color: #206fb1;
	}
	.btnWrap button.btn1:focus {
		background-color: #206fb1;
	}
	.btnWrap button.btn1:active {
		background-color: #206fb1;
	}
	.btnWrap button.btn2 {
		margin-top: 15px;
		color: #206fb1;
		border: 1px solid #206fb1;
		background-color: transparent;
	}
	.btnWrap button.btn2[disabled] {
		background-color: transparent;
	}
	.btnWrap button.btn2:hover {
		background-color: transparent;
	}
	.btnWrap button.btn2:focus {
		color: #206fb1;
		border: 1px solid #206fb1;
		background-color: transparent;
	}
	.btnWrap button.btn2:active {
		color: #206fb1;
		border: 1px solid #206fb1;
		background-color: transparent;
	}
	.inputView {
		flex: 1;
		position: relative;
		font-size: 30rpx;
		color: #333;
		line-height: 2rem;
		padding-left: 10px;
	}
</style>
