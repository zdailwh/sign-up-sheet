<template>
	<view class="resultWrap">
		<view class="iconWrap">
			<icon type="success" size="60"/>
		</view>
		<view class="titleWrap">
			预约成功
		</view>
		<view class="noticeWrap">
			恭喜您预约成功！你的排号码是XX号，<br>请于XX年月日上午携相关材料到校报到。
		</view>
	</view>
</template>

<script>
</script>

<style>
page {
	background-color: #fff;
}
.resultWrap {
	text-align: center;
}
.resultWrap .iconWrap {
	margin: 40px 0 10px;
}
.resultWrap .titleWrap {
	font-size: 40rpx;
	line-height: 60rpx;
	color: #333;
}
.resultWrap .noticeWrap {
	margin: 50rpx 30rpx;
	font-size: 32rpx;
	color: #555;
	line-height: 2;
}
</style>
