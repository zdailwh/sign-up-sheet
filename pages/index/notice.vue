<template>
	<view class="noticeWrap">
		<view class="headTitle">
			晋城爱物学校<br>	2021年一年级新生招生公告
		</view>
		<view class="cont indent">
			根据市政府办、市区规范中小学招生工作领导组及市、区教育局有关公告、文件、会议精神，结合我校实际，现将2021年小学一年级新生招生工作事宜公告如下：
		</view>
		<view class="title indent">一、服务范围</view>
		<view class="cont indent">
			西环路以东；景西路以西；新市西街以南；凤台西街以北
		</view>
		<view class="title indent">二、年龄要求</view>
		<view class="cont indent">
			2015年8月31日前（含8月31日）出生，年满6周岁的适龄儿童（不接受已在其他学校就读并已注册学籍的）。
		</view>
		<view class="title indent">三、验证时间</view>
		<view class="cont indent">
			8月12日—13日；上午8:30—12:00；下午3:00—6:30；如规定时间完不成验证的，工作时间将延长至8月14日上午。
		</view>
		<view class="title indent">四、验证地点</view>
		<view class="cont indent">
			中学部一楼阶梯教室。
		</view>
		<view class="title indent">五、所需证件及材料</view>
		<view class="cont indent">验证时家长须携带<text class="strong">“线下验证码”及以下证件的原件和复印件：</text></view>
		<view class="cont indent">1. 现常住地户口簿（适龄儿童户籍应与父母一致，且须是2021年6月30日前理入的）；</view>
		<view class="cont indent">2. 与常住地户口簿地址一致的房产证明（房产证/购房合同/购房收据等）；</view>
		<view class="cont indent">3. 预防接种证和预防接种情况审核报告单。</view>
		<view class="cont indent">验证后符合入学条件的，由学校进行录取。</view>
		<!-- <view class="title">六、日程安排</view>
		<view class="cont">1.8月12日上午审验1—70序号的相关证件；</view>
		<view class="cont">2.8月12日下午审验71—150序号的相关证件；</view>
		<view class="cont">3.8月13日上午审验151—225序号的相关证件；</view>
		<view class="cont">4.8月13日下午审验226以后序号的相关证件。</view> -->
		<view class="title indent">六、验证程序</view>
		<view class="cont indent">1. 凭在我校“线上预约小程序”上预约的时间段和顺序号入校；</view>
		<view class="cont indent">2. 证件查验；</view>
		<view class="cont indent">3. 注册登记；</view>
		<view class="cont indent">4. 网上信息确认。</view>
		<view class="title indent">七、温馨提示</view>
		<view class="cont indent"><text class="strong">1. 入校验证实行扫码预约，凡符合我校入学条件的都会接收，无需排队。每位儿童名字及对应的身份证号只能预约一次。</text></view>
		<view class="cont indent"><text class="strong">2. 第一监护人</text>（不接受非第一监护人代劳）凭扫码预约成功的截图及提示的序号和时间入校。</view>
		<view class="cont indent">3. 户口簿复印件应将户口薄主页（有地址、派出所公章）、户主页、适龄儿童页复印<text class="strong">在一页A4纸上</text>，所有原件学校留存后将到发证和出具机关（单位）及相关官网复核，开学后发还。</view>
		<view class="cont indent">4. 8月28日上午9点，按照市教育局统一安排，在有关部门及家长代表的监督下进行阳光编班。</view>
		<view class="cont indent">5. 烈士和现役军人子女、市（区）政府引进的高级人才子女等按照有关政策执行。</view>
		<view class="cont indent">6. 根据疫情防控工作需要，入校家长须出示健康码，佩戴口罩，保持安全间距，遵守秩序，禁止吸烟。</view>
		<view class="cont indent"><text class="strong">户籍证明以2021年6月30日理入为准，不接受挂靠户口。凡提供虚假住房及户籍证明骗取入学资格的，除取消入学资格外，报有关部门录入公民个人征信系统。</text></view>
		<view class="cont indent" style="margin-top: 20rpx;"><text class="strong">监督电话：0356-2213568</text></view>
		<view class="cont right" style="margin-top: 50rpx;">二〇二一年八月十日</view>
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
