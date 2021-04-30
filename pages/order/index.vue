<template>
	<view>
		<view class="formTitle">您已成功预约！</view>
		<uni-list>
		    <uni-list-item title="学生姓名" :rightText="order.name"></uni-list-item>
			<uni-list-item title="性别" :rightText="order.sex"></uni-list-item>
			<uni-list-item title="身份证号" :rightText="order.IDCard"></uni-list-item>
			<uni-list-item title="联系电话" :rightText="order.mobile"></uni-list-item>
			<uni-list-item title="户口所在地" :rightText="order.household + ' ' + order.policeStation"></uni-list-item>
			<uni-list-item title="预约时间" :rightText="order.orderDate + ' ' + order.orderTime"></uni-list-item>
		</uni-list>
	</view>
</template>

<script>
export default {
	data() {
		return {
			order:{
				name:'张三',
				sex:'女',
				IDCard: '150550199001010011',
				mobile: '18634567890',
				household: '山西省 太原市 小店区',
				policeStation: 'xx派出所',
				orderDate: '2021-07-25',
				orderTime: '上午'
			}
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
</style>
