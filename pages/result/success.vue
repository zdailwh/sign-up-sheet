<template>
	<view class="resultWrap">
		<view class="iconWrap">
			<icon type="success" size="60"/>
		</view>
		<view class="titleWrap">
			预约成功
		</view>
		<view class="contWrap">
			恭喜您预约成功！你的排队号码是<text class="bold">{{order.no}}</text>号，<br>请于<text class="bold">{{order.apply_date}}{{parseInt(order.am_pm) === 1? '上午' : '下午'}}</text><br>携相关材料到校报到。
			<!-- 恭喜您预约成功！<br>你的排队号码是<text class="bold">7</text>号，<br>请于<text class="bold">2021-08-08 上午 8:30</text><br>携相关材料到校报到。 -->
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				order: {}
			}
		},
	    onLoad: function (option) {
			this.order = JSON.parse(option.order) || {}
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
</style>
