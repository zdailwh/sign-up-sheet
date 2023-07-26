<template>
	<view class="noticeWrap">
		<view class="headTitle">
			晋城市中原街小学<br>2023年招生简章
		</view>
		<view class="cont indent">
			根据晋城市教育局《2023年市区义务教育阶段学校招生实施方案》和《学区划分方案（试行）》文件精神，结合学校实际情况，现将我校2023年一年级新生招生工作相关事项公布如下：
		</view>
		<view class="title indent">一、服务片区</view>
		<view class="cont indent">
			东至泽州路，西至黄华街南段，南至白水西街，北至文昌西街。
		</view>
		<view style="text-align: center;"><image src="../../static/image/map.jpg" mode="widthFix"></image></view>
		<view class="title indent">二、招生对象</view>
		<view class="cont indent">
			在服务片区内实际居住，且年满6周岁（2017年8月31日前出生）的城区户籍适龄儿童和符合条件的进城务工人员随迁子女。
		</view>
		<view class="title indent">三、招生数量</view>
		<view class="cont indent">
			4个教学班，共180人。
		</view>
		<view class="title indent">四、招生原则</view>
		<view class="cont indent">
			1. 划片免试、相对就近及房户一致优先入学原则；
		</view>
		<view class="cont indent">
			2.公开、公平、公正原则；
		</view>
		<view class="cont indent">
			3.总量控制原则。
		</view>
		<view class="title indent">五、报名程序及相关事项</view>
		<view class="cont indent"><text class="strong">（一）线上信息登记：7月11日0时至7月15日24时</text></view>
		<view class="cont indent">请在“晋来办”APP中登录“晋城市小学入学登记系统”进行信息登记，登记成功可自动生成“线下验证码”。监护人要保证信息真实性，凡填报虚假信息和未登记信息的，取消报名资格，后果自负。</view>
		<view class="cont indent"><text class="strong">（二）学校验证录取：分两个阶段进行</text></view>
		<view class="cont indent">1.第一阶段：8月18日至19日，城区户籍适龄儿童。</view>
		<view class="cont indent">请携带<text class="strong">孩子、线下验证码</text>及<text class="strong">以下证件的原件和复印件：</text>（1）户口簿；（2）和常住地户口簿地址一致的房产证明（房产证/购房合同/购房收据/公租房协议及合同等）。验证后符合入学条件的，学校予以录取。</view>
		<view class="cont indent">2.第二阶段：8月22日至23日，符合条件的进城务工人员随迁子女。</view>
		<view class="cont indent">请携带<text class="strong">孩子、线下验证码</text>及<text class="strong">以下证件的原件和复印件：</text>（1）户口簿；（2）父母双方或法定监护人居住证明材料（居住证/居住登记证明）；（3）父母双方或法定监护人创业就业情况证明材料（创业就业证/劳动合同备案表/营业执照等）。验证后符合入学条件的，由学校根据空余学位情况，按照《居住证》等有效证件签发时间顺序进行录取。未被录取的学生请到相对就近学校就读。</view>
		<view class="cont indent"><text class="strong">（三）阳光分班：8月28日上午，对录取学生进行电脑配位、阳光分班。</text></view>
		<view class="title indent">六、其他</view>
		<view class="cont indent">（一）审核工作时间：上午8:30—11:30  下午3:30—6:30</view>
		<view class="cont indent">（二）审核地点：晋城市中原街小学院内</view>
		<view class="cont indent">（三）咨询电话：2213290      举报电话：2087671</view>
		<view class="cont indent">（四）更多详细资讯请扫码关注“中原街小学公众号”获取。</view>
		
		<view class="cont right" style="margin-top: 50rpx;">2023年8月16日</view>
	</view>
</template>

<script>
export default {
	onLoad() {
		uni.showShareMenu()
	}
}
</script>

<style>
.indent {
	text-indent: 2em;
}
.noticeWrap {
	padding: 30rpx 40rpx;
}
.noticeWrap .headTitle {
	font-size: 40rpx;
	color: #333;
	text-align: center;
	margin-bottom: 30rpx;
}
.noticeWrap .cont {
	font-size: 30rpx;
	line-height: 1.7;
	color: #555;
}
.noticeWrap .title {
	font-size: 35rpx;
	line-height: 2;
	color: #333;
	font-weight: bold;
	margin: 20rpx 0;
}
.right {
	text-align: right;
}
.strong {
	font-weight: bold;
	color: #222;
}
</style>
