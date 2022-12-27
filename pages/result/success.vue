<template>
	<view class="resultWrap">
		<view class="iconWrap">
			<icon type="success" size="60"/>
		</view>
		<view class="titleWrap">
			预约成功
		</view>
		<view class="contWrap">
			恭喜您预约成功！您的排队号码是<text class="bold">{{order.no}}</text>号，<br>请于<text class="bold">{{order.apply_date}} {{parseInt(order.am_pm) === 1? '上午' : '下午'}} {{order.begintime}}</text><br>携相关证件到校验证。
			<!-- 恭喜您预约成功！<br>您的排队号码是<text class="bold">7</text>号，<br>请于<text class="bold">2021-08-08 上午 8:30</text><br>携相关证件到校验证。 -->
		</view>
		<view class="importNotice">
			提示：请截屏并携带相关资料提前五分钟到爱物学校门口等候。
		</view>
		<view class="notice">
			查看<navigator url="/pages/order/index" open-type="switchTab" hover-class="none" class="link">我的预约 </navigator>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				am_begin: '8:30',
				pm_begin: '15:00',
				gap: 3,
				order: {}
			}
		},
	    onLoad: function (option) {
			uni.showShareMenu()
			this.order = JSON.parse(option.order) || {}
			if (this.order.id) {
				this.calcBegintime()
			}
	    },
		methods: {
			calcBegintime() {
				if (this.order.am_pm === 1) {
					var minit = (this.order.no - 1) * this.gap
					var begintime = this.minitPlus(this.am_begin, minit)
				} else if (this.order.am_pm === 2) {
					var minit = (this.order.no - 1) * this.gap
					var begintime = this.minitPlus(this.pm_begin, minit)
				}
				this.order.begintime = begintime
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
				console.log(h + ':' + this.padStart(m))
				return h + ':' + this.padStart(m)
			},
			padStart(val) {
				if (val < 10) {
					return '0' + val
				} else {
					return val
				}
			}
		}
	}
</script>

<style>
page {
	background-color: #9fd7e6;
}
.resultWrap {
	text-align: center;
}
.resultWrap .iconWrap {
	margin: 70rpx 0 30rpx;
}
.resultWrap .titleWrap {
	font-size: 40rpx;
	line-height: 60rpx;
	color: #333;
}
.resultWrap .contWrap {
	margin: 50rpx 30rpx;
	font-size: 35rpx;
	color: #555;
	line-height: 2;
}
.resultWrap .contWrap .bold {
	font-size: 50rpx;
	font-weight: bold;
	color: orange;
	margin: 0 10rpx;
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
	margin: 0 10rpx;
}
.importNotice {
	margin: 30rpx;
	font-size: 30rpx;
	color: #f00;
}
</style>
