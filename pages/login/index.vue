<template>
	<view class="loginWrap">
		<view class='header'>
			<image src='../../static/image/wx_login.png'></image>
		</view>
		<view class='content'>
			<view>申请获取以下权限</view>
			<text>获得你的公开信息(昵称，头像、地区等)</text>
		</view>
		<button class='bottom' type='primary' open-type="getUserInfo" withCredentials="true" lang="zh_CN" @getuserinfo="wxGetUserInfo">授权登录</button>
	</view>
</template>

<script>
export default {
	data() {
		return {
			SessionKey: '',
			OpenId: '',
			isCanUse: uni.getStorageSync('isCanUse') || true, // 默认为true
		}
	},
	onLoad() {//默认加载
		this.login()
	},
	methods: {
		//第一授权获取用户信息===》按钮触发
		wxGetUserInfo() {
			let _this = this
			uni.getUserInfo({
				provider: 'weixin',
				success: function(infoRes) {
					let nickName = infoRes.userInfo.nickName //昵称
					let avatarUrl = infoRes.userInfo.avatarUrl //头像
					try {
						uni.setStorageSync('isCanUse', false) //记录是否第一次授权  false:表示不是第一次授权
						// _this.updateUserInfo()
					} catch (e) {}
				},
				fail(res) {}
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
					console.log('login code' + code)
					if (!_this.isCanUse) {
						//非第一次授权获取用户信息
						uni.getUserInfo({
							provider: 'weixin',
							success: function(infoRes) {
	　　　　　　　　　　　　　//获取用户信息后向调用信息更新方法
								let nickName = infoRes.userInfo.nickName //昵称
								let avatarUrl = infoRes.userInfo.avatarUrl //头像
								// _this.updateUserInfo() //调用更新信息方法
							}
						})
					}
					//2.将用户登录code传递到后台置换用户SessionKey、OpenId等信息
					// uni.request({
					// 	url: 'https://awxx.jc114.com/user/login',
					// 	data: {
					// 		code: code,
					// 	},
					// 	method: 'POST',
					// 	header: {
					// 		'content-type': 'application/json'
					// 	},
					// 	success: (res) => {
					// 		console.log('login response')
					// 		console.log(res)
					// 		//openId、或SessionKdy存储//隐藏loading
					// 		uni.hideLoading()
					// 	},
					// 	fail: (res) => {
					// 		uni.hideLoading()
					// 		console.log(res.errMsg)
					// 	}
					// })
				}
			})
		},
		 //向后台更新信息
		updateUserInfo() {
			let _this = this
			uni.request({
				url:'url' ,//服务器端地址
				data: {
					appKey: this.$store.state.appKey,
					customerId: _this.customerId,
					nickName: _this.nickName,
					headUrl: _this.avatarUrl
				},
				method: 'POST',
				header: {
					'content-type': 'application/json'
				},
				success: (res) => {
					if (res.data.state == "success") {
						uni.reLaunch({//信息更新成功后跳转到小程序首页
							url: '/pages/index/index'
						})
					}
				}
			})
		}
	}
}
</script>

<style>
</style>
