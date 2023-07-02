<template>	
	<view>
		<view class="logoWrap">
			<image src="../../static/image/logo.png" mode="widthFix"></image>
		</view>
		<uni-forms :value="formData" ref="form" labelWidth="80">
			<view class="formTitle">请如实填写以下信息</view>
			<view class="formWrap">
				<uni-forms-item label="学生姓名" name="name" label-align="left">
					<uni-easyinput required type="text" v-model="formData.name" placeholder="请输入学生姓名" />
				</uni-forms-item>
				<uni-forms-item name="sex" label="性别" label-align="left">
					<uni-data-checkbox v-model="formData.sex" :localdata="sexArr"/>
				</uni-forms-item>
				<uni-forms-item label="身份证号码" name="idc_no" label-align="left">
					<uni-easyinput required type="idcard" v-model="formData.idc_no" placeholder="请输入身份证号码" />
				</uni-forms-item>
				<uni-forms-item label="联系电话" name="contract_number" label-align="left">
					<uni-easyinput required type="number" v-model="formData.contract_number" placeholder="联系电话" />
				</uni-forms-item>
				<uni-forms-item label="户籍所在地" name="household" label-align="left">
					<!-- <uni-easyinput required type="text" v-model="formData.household" placeholder="请选择户籍所在地" @focus="openPicker()"/> -->
					<view class="inputView" @click="openPicker()">{{formData.household}}</view>
				</uni-forms-item>
				<uni-forms-item label="家庭住址" name="officer_name" label-align="left">
					<uni-easyinput required type="text" v-model="formData.officer_name" placeholder="地址填写必须和户口本上完全一致" />
				</uni-forms-item>
			</view>
			<view class="formTitle">您预约的验证日期及时段为：</view>
			<view class="formWrap">
				<template v-if="afterNums.id">
					<view class="applyDateWrap">
						{{afterNums.day_num}} {{parseInt(afterNums.am_pm) === 1? '上午' : '下午'}}
					</view>
					<view v-if="new Date().getTime() < afterNums.can_apply * 1000" class="canApplyNotice">
						<view>还没有到放号时间，请{{afterNums.can_apply | formatCanApply}}再来</view>
					</view>
					<view v-else-if="afterNums.useful_nums === 0" class="canApplyNotice">
						<view>该时段没有剩余号源，请下个时段再来吧！</view>
					</view>
				</template>
				<view v-else class="canApplyNotice" style="text-align: left;">
					<view>如多次预约仍未成功，请于8月20日上午8:30携带相关材料直接到校验证。</view>
				</view>
			</view>
			<!-- <view class="formWrap">
				<view class="applyDateWrap">
					{{formData.apply_date | dateFormat}} {{parseInt(formData.am_pm) === 1? '上午' : '下午'}}
				</view>
				<view class="canApplyNotice">
					<template v-if="new Date().getTime() < canApplyRes.can_apply * 1000">
						<view>还没有到放号时间，请{{canApplyRes.can_apply | formatCanApply}}再来</view>
					</template>
					<template v-else-if="canApplyRes.useful_nums === 0">
						<view>该时段没有剩余号源，请下个时段再来吧！</view>
					</template>
				</view>
			</view> -->
			<!-- <view class="formWrap">
				<uni-forms-item label="报名日期" name="apply_date" label-align="left">
					<uni-easyinput required v-model="formData.apply_date" disabled="true" placeholder="报名日期" />
				</uni-forms-item>
				<uni-forms-item label="报名日期" name="apply_date" label-align="left">
					<picker mode="date" :value="formData.apply_date" :start="startDate" :end="endDate" @change="bindDateChange">
						<view class="dateWrap"><text v-if="formData.apply_date">{{formData.apply_date}}</text><text style="color: grey;" v-else>请选择日期</text></view>
					</picker>
				</uni-forms-item>
				<uni-forms-item name="am_pm" label="时间段" label-align="left">
					<uni-data-checkbox v-model="formData.am_pm" :localdata="am_pmArr"/>
				</uni-forms-item>
			</view> -->
		</uni-forms>
		<view class="btnWrap">
			<button type="primary" :disabled="applyDisabled" @click="submitForm">预约</button>
		</view>
		<view class="notice">
			查看<navigator url="/pages/index/notice" hover-class="none" class="link">《招生简章》</navigator>
		</view>
	
		<lotus-address v-on:choseVal="choseValue" :lotusAddressData="lotusAddressData"></lotus-address>
		
		<uni-popup ref="popup" type="center" :maskClick="false">
			<view class="noticeWrap">
				<view class="noticeTitle">预约须知</view>
				<view class="noticeMsg">
					<view class="noticeItem">1. 身份证号码、手机号等个人敏感信息仅用于晋城爱物学校学生排号，并保证绝不会泄露您的信息。</view>
					<view class="noticeItem">2. 请仔细阅读<navigator url="/pages/index/notice" hover-class="none" class="link">《招生简章》</navigator></view>
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
var temp = {
	am_pm: 1,
	apply_date: "2021-05-03",
	area: "城区",
	city: "晋城市",
	contract_number: "18601949678",
	created_at: null,
	id: 5,
	idc_no: "140430201404083223",
	name: "赵丹",
	no: 3,
	officer_name: "东西派出所",
	province: "山西省",
	sex: 2,
	updated_at: null,
	user_id: 2
}
export default {
	components:{
	    "lotus-address":lotusAddress
	},
	filters: {
		dateFormat(val) {
			if (val === '') return ''
			const date = new Date(val)
			const formatObj = {
				y: date.getFullYear(),
				m: date.getMonth() + 1,
				d: date.getDate()
			}
			return formatObj.y + '年' + formatObj.m + '月' + formatObj.d + '日'
		},
		formatCanApply(val) {
			if (!val) return ''
			const date = new Date(val * 1000)
			const formatObj = {
				y: date.getFullYear(),
				m: date.getMonth() + 1,
				d: date.getDate(),
				h: date.getHours(),
				i: date.getMinutes(),
				s: date.getSeconds()
			}
			return formatObj.y + '-' + formatObj.m.toString().padStart(2, '0') + '-' + formatObj.d.toString().padStart(2, '0') + ' ' + formatObj.h.toString().padStart(2, '0') + ':' + formatObj.i.toString().padStart(2, '0') + ':' + formatObj.s.toString().padStart(2, '0')
		}
	},
	computed: {
		applyDisabled () {
			return (new Date().getTime() < this.afterNums.can_apply * 1000) || (new Date().getTime() >= this.afterNums.can_apply * 1000 && this.afterNums.useful_nums === 0) || !this.afterNums.id
		}
	},
	data() {
		return {
			agree: [],
			canApplyRes: {},
			afterNums: {},
			lotusAddressData:{
				visible:false,
				provinceName:'',
				cityName:'',
				townName:'',
			},
			formData:{
				name:'',
				sex:'',
				idc_no: '',
				contract_number: '',
				household: '山西省 晋城市 城区',
				province: "山西省",
				city: "晋城市",
				area: "城区",
				officer_name: '',
				apply_date: '',
				am_pm: ''
			},
			sexArr: [{
				text: '男',
				value: 1
			}, {
				text: '女',
				value: 2
			}],
			am_pmArr: [{
				text: '上午',
				value: 1
			}, {
				text: '下午',
				value: 2
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
				idc_no: {
					rules: [
						{ required: true, errorMessage: '请输入身份证号码' },
						{
							validateFunction:function(rule,value,data,callback) {
								var re15 = /^[1-9]\d{7}((0\d)|(1[0-2]))(([0|1|2]\d)|3[0-1])\d{3}$/
								var re18 = /^[1-9]\d{5}[1-9]\d{3}((0\d)|(1[0-2]))(([0|1|2]\d)|3[0-1])\d{3}([0-9]|X)$/
								var res = (re15.test(value) || re18.test(value))
								if (res==false) {
									callback('身份证号码格式错误')
								}
								var birthday = getBirthdayFromIdCard(value)
								var y = birthday.substr(0,4)
								var m = birthday.substr(4,2)
								var d = birthday.substr(6,2)
								var birTime = new Date(y + '/' + m + '/' + d).getTime()
								if (birTime < new Date('2016/09/01').getTime() || birTime > new Date('2017/08/31').getTime()) {
									callback('出生日期不在2016-09-01至2017-08-31内')
								}
								return true
							}
						}
					]
				},
				contract_number: {
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
						{ required: true, errorMessage: '请选择户籍所在地' }
					]
				},
				officer_name: {
					rules: [
						{ required: true, errorMessage: '请输入家庭住址' }
					]
				},
				apply_date: {
					rules: [
						{ required: true, errorMessage: '请选择预约报名日期' }
					]
				},
				am_pm: {
					rules: [
						{ required: true, errorMessage: '请选择预约报名时间段' }
					]
				},
			},
			startDate: '',
			endDate: ''
		}
	},
	onLoad() {
		uni.showShareMenu()
		this.formInit()
	},
	onReady() {
		// 显示招生公告
		this.open()
		this.$refs.form.setRules(this.rules)
	},
	onShow() {
		this.afterNums = {}
		if (!uni.getStorageSync('session_key') || !uni.getStorageSync('user_id')) {
			console.log('去登录')
			this.login()
		} else {
			// 获得指定日期当天或者之后的第一条可用预约记录
			this.getAfterNums()
		}
	},
	methods: {
		formInit() {
			this.$refs.form.clearValidate()
			// this.formData = {
			// 	name:'',
			// 	sex:'',
			// 	idc_no: '',
			// 	contract_number: '',
			// 	household: '山西省 晋城市 城区',
			// 	province: "山西省",
			// 	city: "晋城市",
			// 	area: "城区",
			// 	officer_name: '',
			// 	apply_date: '',
			// 	am_pm: ''
			// }
			// 设置地址
			this.lotusAddressData.provinceName = '山西省'
			this.lotusAddressData.cityName = '晋城市'
			this.lotusAddressData.townName = '城区'
			
			// 设置预约时间和时段
			// var today = new Date().toLocaleDateString()
			// var tomorrow = new Date(new Date(today).getTime() + 24 * 60 * 60 * 1000).toLocaleDateString()
			// var nowH = new Date().getHours()
			// if (nowH < 12) {
			// 	// 12点前约今天下午的号
			// 	this.formData.am_pm = 2
			// 	this.formData.apply_date = this.parseTime(today)
			// 	this.startDate = today
			// 	this.endDate = today
			// } else {
			// 	// 12点后约明天上午的号
			// 	this.formData.am_pm = 1
			// 	this.formData.apply_date = this.parseTime(tomorrow)
			// 	this.startDate = tomorrow
			// 	this.endDate = tomorrow
			// }
			
			// // 获取某日期时间段的放号情况
			// this.getIfCanApply(this.formData.apply_date, this.formData.am_pm)
		},
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
			// uni.navigateTo({
			// 	url: '/pages/index/confirm?formData=' + JSON.stringify(temp)
			// })
			// 手动提交表单
			this.$refs.form.submit().then((res)=>{
				var formdata = this.formData
				formdata.user_id = uni.getStorageSync('user_id')
				uni.navigateTo({
					url: '/pages/index/confirm?formData=' + JSON.stringify(formdata)
				})
			}).catch(err =>{
            })
		},
		//打开picker
		openPicker() {
			this.lotusAddressData.visible = true
		},
		//回传已选的省市区的值
		choseValue(res){
			//res数据源包括已选省市区与省市区code
			this.lotusAddressData.visible = res.visible //visible为显示与关闭组件标识true显示false隐藏
			//res.isChose = 1省市区已选 res.isChose = 0;未选
			if(res.isChose){
				this.lotusAddressData.provinceName = res.province //省
				this.lotusAddressData.cityName = res.city //市
				this.lotusAddressData.townName = res.town //区
				this.formData.household = `${res.province} ${res.city} ${res.town}` //region为已选的省市区的值
				this.formData.province = res.province //省
				this.formData.city = res.city //市
				this.formData.area = res.town //区
			}
		},
		bindDateChange: function(e) {
			this.formData.apply_date = e.target.value
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
		},
		getIfCanApply(date, am_pm) {
			uni.request({
				url: 'https://school.jcawxx.cn/admin/apply_detail?date=' + date + '&am_pm=' + am_pm, 
				method: 'GET',
				header: {
					'token': uni.getStorageSync('session_key') //自定义请求头信息
				},
				success: (res) => {
					if (res.data.httpCode === 200) {
						this.canApplyRes = res.data.data
						console.log(this.canApplyRes)
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
					console.log(err)
					uni.showToast({
						title: '请求出错，请稍后再试！',
						icon: 'none',
						duration: 3000
					})
				}
			})
		},
		getAfterNums() {
			var today = new Date().toLocaleDateString()
			uni.request({
				url: 'https://school.jcawxx.cn/apply/after_nums?date=' + today, 
				method: 'GET',
				header: {
					'token': uni.getStorageSync('session_key') //自定义请求头信息
				},
				success: (res) => {
					if (res.data.httpCode === 200) {
						this.afterNums = res.data.data
						this.formData.am_pm = this.afterNums.am_pm
						this.formData.apply_date = this.afterNums.day_num
					} else if (res.data.message === '提供的信息已经过期，请重新登录') {
						this.login()
					} else {
						// uni.showToast({
						// 	title: res.data.message,
						// 	icon: 'none',
						// 	duration: 3000
						// })
					}
				},
				fail: (err) => {
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
						url: 'https://school.jcawxx.cn/user/login',
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
							_this.getAfterNums()
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
function getBirthdayFromIdCard(idCard) {
	var birthday = ''
	if(idCard !== null && idCard !== '') {  
		if(idCard.length === 15) {  
			birthday = '19'+idCard.substr(6, 6)
		} else if (idCard.length === 18) {  
			birthday = idCard.substr(6, 8)
		}
	}
	return birthday
}
</script>

<style>
page {
	background-color: #9fd7e6;
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
	color: #666;
}
.formWrap {
	padding: 15px 15px 0 15px;
	/* background-color: rgba(159,215,230,.5); */
	background-color: rgba(255,255,255,.7);
}
.btnWrap {
	padding: 15px;
}
.btnWrap button, .buttonWrap button {
	background-color: #206fb1;
}
.btnWrap button[disabled], .buttonWrap button[disabled] {
	background-color: #206fb1;
}
.btnWrap button:hover, .buttonWrap button:hover {
	background-color: #206fb1;
}
.btnWrap button:focus, .buttonWrap button:focus {
	background-color: #206fb1;
}
.btnWrap button:active, .buttonWrap button:active {
	background-color: #206fb1;
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
	line-height: 1.7;
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

.inputView {
	flex: 1;
	position: relative;
	font-size: 30rpx;
	color: #333;
	line-height: 2.5rem;
	border: 1px solid #e5e5e5;
	border-radius: 4px;
	padding-left: 10px;
}

.notice {
	margin: 10rpx 0 30rpx;
	text-align: center;
	padding: 0 30rpx;
	font-size: 30rpx;
	color: #333;
	line-height: 2;
}
.link {
	display: inline-block;
	color: #007AFF;
}
.logoWrap {
	
}
.logoWrap image {
	display: block;
	width: 450rpx;
	margin: 20rpx auto;
}
.applyDateWrap {
	text-align: center;
	padding-bottom: 10px;
	font-size: 40rpx;
	color: #333;
}
.canApplyNotice {
	text-align: center;
	padding-bottom: 15px;
	font-size: 28rpx;
	color: #f00;
}
</style>
