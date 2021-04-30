<template>
	<view v-if="isCanUse" class="loginWrap">
		<view class='header'>
			<image src='../../static/image/wx_login.png'></image>
		</view>
		<view class='content'>
			<view>申请获取以下权限</view>
			<text>获得你的公开信息(昵称，头像、地区等)</text>
		</view>
		<button class='bottom' type='primary' open-type="getUserInfo" withCredentials="true" lang="zh_CN" @getuserinfo="wxGetUserInfo">授权登录</button>
	</view>
	<view v-else>
		<uni-forms :value="formData" ref="form">
			<view class="formTitle">请如实填写以下信息</view>
			<view class="formWrap">
				<uni-forms-item label="学生姓名" name="name" label-align="left">
					<uni-easyinput required type="text" v-model="formData.name" placeholder="请输入学生姓名" />
				</uni-forms-item>
				<uni-forms-item name="sex" label="性别" label-align="left">
					<uni-data-checkbox v-model="formData.sex" :localdata="sexArr"/>
				</uni-forms-item>
				<uni-forms-item label="身份证号" name="IDCard" label-align="left">
					<uni-easyinput required type="idcard" v-model="formData.IDCard" placeholder="请输入身份证号" />
				</uni-forms-item>
				<uni-forms-item label="联系电话" name="mobile" label-align="left">
					<uni-easyinput required type="number" v-model="formData.mobile" placeholder="联系电话" />
				</uni-forms-item>
				<uni-forms-item label="户口" name="household" label-align="left">
					<uni-easyinput required type="text" v-model="formData.household" placeholder="请选择户口所在地" @focus="openPicker()"/>
				</uni-forms-item>
				<uni-forms-item label="派出所" name="policeStation" label-align="left">
					<uni-easyinput required type="text" v-model="formData.policeStation" placeholder="请输入户口所在派出所" />
				</uni-forms-item>
			</view>
			<view class="formTitle">请选择预约报名日期</view>
			<view class="formWrap">
				<uni-forms-item label="报名日期" name="orderDate" label-align="left">
					<uni-easyinput required v-model="formData.orderDate" disabled="true" placeholder="报名日期" />
				</uni-forms-item>
				<!-- <uni-forms-item label="报名日期" name="orderDate" label-align="left">
					<picker mode="date" :value="formData.orderDate" start="2021-07-25" end="2021-08-25" @change="bindDateChange">
						<view class="dateWrap"><text v-if="formData.orderDate">{{formData.orderDate}}</text><text style="color: grey;" v-else>请选择日期</text></view>
					</picker>
				</uni-forms-item> -->
				<uni-forms-item name="orderTime" label="时间段" label-align="left">
					<uni-data-checkbox v-model="formData.orderTime" :localdata="orderTimeArr"/>
				</uni-forms-item>
			</view>
		</uni-forms>
		<view class="btnWrap">
			<button type="primary" @click="submitForm">提交</button>
		</view>		
	
		<lotus-address v-on:choseVal="choseValue" :lotusAddressData="lotusAddressData"></lotus-address>
		
		<uni-popup ref="popup" type="center" :maskClick="false">
			<view class="noticeWrap">
				<view class="noticeTitle">招生公告</view>
				<view class="noticeMsg">
					<view class="noticeItem">1、请认真阅读</view>
					<view class="noticeItem">2、请认真阅读以下信息请认真阅读以下信息请认真阅读以下信息请认真阅读以下信息请认真阅读以下信息</view>
				</view>
				<view class="agreeWrap">
					<checkbox-group @change="binddata('agree', $event.detail.value)">
						<label>
							<checkbox value="agreetrue" /><text>我已阅读并同意</text>
						</label>
					</checkbox-group>
				</view>
				<view class="buttonWrap">
					<button class="uni-button uni-button-full" :disabled="!agree.length || agree.indexOf('agreetrue') === -1" type="primary" @click="closeNotice">去预约</button>
				</view>
			</view>
		</uni-popup>
	</view>
</template>

<script>
import lotusAddress from "../../components/Winglau14-lotusAddress/Winglau14-lotusAddress.vue"
export default {
	components:{
	    "lotus-address":lotusAddress
	},
	data() {
		return {
			agree: [],
			SessionKey: '',
			OpenId: '',
			nickName: null,
			avatarUrl: null,
			isCanUse: uni.getStorageSync('isCanUse') || false, // 默认为true
			lotusAddressData:{
				visible:false,
				provinceName:'',
				cityName:'',
				townName:'',
			},
			formData:{
				name:'',
				sex:'',
				IDCard: '',
				mobile: '',
				household: '',
				policeStation: '',
				orderDate: '',
				orderTime: ''
			},
			sexArr: [{
				text: '男',
				value: 1
			}, {
				text: '女',
				value: 2
			}],
			orderTimeArr: [{
				text: '上午',
				value: '上午'
			}, {
				text: '下午',
				value: '下午'
			}],
			rules: {
				name: {
					rules: [
						{ required: true, errorMessage: '请输入学生姓名' }
						// { minLength: 3, maxLength: 5, errorMessage: '姓名长度在 {minLength} 到 {maxLength} 个字符' }
					]
				},
				sex: {
					rules: [
						{ required: true, errorMessage: '请选择性别' }
					]
				},
				IDCard: {
					rules: [
						{ required: true, errorMessage: '请输入身份证号' },
						{
							validateFunction:function(rule,value,data,callback) {
								var re15 = /^[1-9]\d{7}((0\d)|(1[0-2]))(([0|1|2]\d)|3[0-1])\d{3}$/
								var re18 = /^[1-9]\d{5}[1-9]\d{3}((0\d)|(1[0-2]))(([0|1|2]\d)|3[0-1])\d{3}([0-9]|X)$/
								var res = (re15.test(value) || re18.test(value))
								if (res==false) {
									callback('身份证号格式错误')
								}
								return true
							}
						}
					]
				},
				mobile: {
					rules: [
						{ required: true, errorMessage: '请输入联系电话' },
						{
							validateFunction:function(rule,value,data,callback) {
								var mob = /^1[3-9]\d{9}$/
								var tel = /^\d{3,4}-\d{7,8}(-\d{3,4})?$/
								var res = (mob.test(value) || tel.test(value))
								if (res==false) {
									callback('手机号或座机号格式错误')
								}
								return true
							}
						}
					]
				},
				household: {
					rules: [
						{ required: true, errorMessage: '请选择户口所在地' }
					]
				},
				policeStation: {
					rules: [
						{ required: true, errorMessage: '请输入户口所在派出所' }
					]
				},
				orderDate: {
					rules: [
						{ required: true, errorMessage: '请选择预约报名日期' }
					]
				},
				orderTime: {
					rules: [
						{ required: true, errorMessage: '请选择预约报名时间段' }
					]
				},
			}
		}
	},
	onReady() {
		// 显示招生公告
		// this.open()
		var today = new Date().toLocaleDateString()
		var tomorrow = new Date(new Date(today).getTime() + 24 * 60 * 60 * 1000).toLocaleDateString()
		console.log(this.parseTime(today) + '/' + this.parseTime(tomorrow))
		this.formData.orderDate = this.parseTime(tomorrow)
		var nowH = new Date().getHours()
		if (nowH < 13) {
			this.formData.orderTime = '上午'
		} else {
			this.formData.orderTime = '下午'
		}

		this.$refs.form.setRules(this.rules)
	},
	onLoad() {//默认加载
		this.login()
	},
	methods: {
		binddata(key, val) {
			this[key] = val
		},
		open(){
			// 通过组件定义的ref调用uni-popup方法
			this.$refs.popup.open()
		},
		closeNotice() {
			if (this.agree.length && this.agree.indexOf('agreetrue') !== -1) {
				this.$refs.popup.close()
			}
		},
		submitForm(form) {
			// 手动提交表单
			this.$refs.form.submit().then((res)=>{
				console.log('表单返回得值：', res)
				uni.showLoading({
					title: '正在提交'
				})
				setTimeout(function () {
				    uni.hideLoading()
					uni.redirectTo({
						url: '/pages/result/success'
					})
				}, 2000)
			}).catch(err =>{
                console.log('表单错误信息：', err);
            })
		},
		//打开picker
		openPicker() {
			this.lotusAddressData.visible = true
			this.lotusAddressData.provinceName = '山西省'
			this.lotusAddressData.cityName = '晋城市'
			this.lotusAddressData.townName = '城区'
		},
		//回传已选的省市区的值
		choseValue(res){
			//res数据源包括已选省市区与省市区code
			console.log(res);
			this.lotusAddressData.visible = res.visible //visible为显示与关闭组件标识true显示false隐藏
			//res.isChose = 1省市区已选 res.isChose = 0;未选
			if(res.isChose){
				this.lotusAddressData.provinceName = res.province //省
				this.lotusAddressData.cityName = res.city //市
				this.lotusAddressData.townName = res.town //区
				this.formData.household = `${res.province} ${res.city} ${res.town}` //region为已选的省市区的值
			}
		},
		bindDateChange: function(e) {
			this.formData.orderDate = e.target.value
		},
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
						_this.updateUserInfo()
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
					if (!_this.isCanUse) {
						//非第一次授权获取用户信息
						uni.getUserInfo({
							provider: 'weixin',
							success: function(infoRes) {
	　　　　　　　　　　　　　//获取用户信息后向调用信息更新方法
								let nickName = infoRes.userInfo.nickName //昵称
								let avatarUrl = infoRes.userInfo.avatarUrl //头像
								_this.updateUserInfo() //调用更新信息方法
							}
						})
					}
					//2.将用户登录code传递到后台置换用户SessionKey、OpenId等信息
					uni.request({
						url: '服务器地址',
						data: {
							code: code,
						},
						method: 'GET',
						header: {
							'content-type': 'application/json'
						},
						success: (res) => {
							//openId、或SessionKdy存储//隐藏loading
							uni.hideLoading()
						},
						fail: (res) => {
							uni.hideLoading()
							console.log(res.errMsg)
						}
					})
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
		},
		parseTime(time) {
		  const date = new Date(time)
		
		  const formatObj = {
		    y: date.getFullYear(),
		    m: date.getMonth() + 1,
		    d: date.getDate(),
		    h: date.getHours(),
		    i: date.getMinutes(),
		    s: date.getSeconds(),
		    a: date.getDay(),
		    ms: date.getMilliseconds()
		  }
		
		  return `${formatObj.y}-${formatObj.m.toString().padStart(2, '0')}-${formatObj.d.toString().padStart(2, '0')}`
		}
	}
}
</script>

<style>
page {
	background-color: #f5f5f5;
}
.uni-forms-item__label .label-text {
	font-size: 30rpx !important;
}
.uni-easyinput__content-input {
	font-size: 30rpx !important;
}
.checklist-text {
	font-size: 30rpx !important;
}
input {
	height: 2.5rem !important;
}
.formTitle {
	padding-left: 30rpx;
	height: 100rpx;
	line-height: 100rpx;
	font-size: 32rpx;
	color: #000;
}
.formWrap {
	padding: 15px 15px 0 15px;
	background-color: #fff;
}
.btnWrap {
	padding: 15px;
}
.dateWrap {
	display: flex;
	height: 2.5rem;
	line-height: 2.5rem;
	padding-left: 10px;
	border: 1px solid #e5e5e5;
	border-radius: 4px;
	color: #333;
	font-size: 30rpx !important;
}
.loginWrap .header {
	margin: 90rpx 0 90rpx 50rpx;
	border-bottom: 1px solid #ccc;
	text-align: center;
	width: 650rpx;
	height: 300rpx;
	line-height: 450rpx;
}

.loginWrap .header image {
	width: 200rpx;
	height: 200rpx;
}

.loginWrap .content {
	margin-left: 50rpx;
	margin-bottom: 90rpx;
}

.loginWrap .content text {
	display: block;
	color: #9d9d9d;
	margin-top: 40rpx;
}

.loginWrap .bottom {
	border-radius: 80rpx;
	margin: 70rpx 50rpx;
	font-size: 35rpx;
}

.noticeWrap {
	width: 600rpx;
	padding: 30rpx;
	border-radius: 10rpx;
	background-color: #fff;
}
.noticeWrap .noticeTitle {
	font-size: 38rpx;
	line-height: 2;
	text-align: center;
}
.noticeWrap .noticeMsg {
	font-size: 30rpx;
	color: #666;
	line-height: 1.5;
	margin-bottom: 20rpx;
}
.noticeWrap .noticeMsg .noticeItem {
	
}
.noticeWrap .agreeWrap {
	font-size: 30rpx;
	color: #666;
	margin: 20rpx auto;
	text-align: center;
}
.noticeWrap .buttonWrap {
	margin: 15rpx auto;
}
</style>
