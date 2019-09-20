<template>
	<view>
		
		<view class="detailxqBan">
			<image src="../../static/images/details-img1.png" mode=""></image>
		</view>
		
		<view class="detail_one">
			<view class="flex">
				<view class="price font14">56.4</view>
				<view class="yuanJia">66.8</view>
			</view>
			<view class="flexRowBetween color2 font13 number">
				<view class="">库存：2133</view>
				<view class="">销量：562</view>
			</view>
			<view class="tit font14">梨子新鲜2件带项10斤陕西早酥梨当季梨皇冠香梨应季水果包邮</view>
		</view>
		<view class="f5H10"></view>
		<view class="detail_join">
			<view class="flexRowBetween">
				<view class="font14">45人正在拼团，可直接参与</view>
				<view class="font13 color3" @click="webSelf.$Router.navigateTo({route:{path:'/pages/goodsDetail_pintuan/goodsDetail_pintuan'}})">查看更多&gt;</view>
			</view>
			<view class="twolist">
				<view class="item flexRowBetween">
					<view class="ll flex">
						<view class="photo">
							<image src="../../static/images/about-img1.png" mode=""></image>
						</view>
						<view class="tit avoidOverflow">名称名称名称</view>
					</view>
					<view class="rr flexRowBetween">
						<view class="downTime">
							<view class="red font14">还差1人拼成</view>
							<view class="time">剩余23:23:48</view>
						</view>
						<view class="go" @click="goPinDan">去拼团</view>
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll flex">
						<view class="photo">
							<image src="../../static/images/about-img1.png" mode=""></image>
						</view>
						<view class="tit avoidOverflow">名称名称名称</view>
					</view>
					<view class="rr flexRowBetween">
						<view class="downTime">
							<view class="red font14">还差1人拼成</view>
							<view class="time">剩余23:23:48</view>
						</view>
						<view class="go" @click="goPinDan">去拼团</view>
					</view>
				</view>
			</view>
		</view>
		<view class="f5H10"></view>
		<view class="infor-title">
			<view class="tt">详情介绍</view>
		</view>
		<view class="detailTexCont">
			<view class="center">
				<view>详情介绍详情介绍详情介绍详情介绍详情</view>
				<view>详情介绍详情介绍详情介绍详情</view>
				<view>详情介绍详情介绍详情介绍详情介绍详情介绍详情</view>
				<view>详情介绍详情介绍详情介绍</view>
				<view>
					<image src="../../static/images/details-img2.png" mode="widthFix"></image>
				</view>
				<view>详情介绍详情介绍详情介绍详情介绍详情</view>
				<view>详情介绍详情介绍详情介绍详情</view>
				<view>详情介绍详情介绍详情介绍详情介绍详情介绍详情</view>
				<view>详情介绍详情介绍详情介绍</view>
				<view>
					<image src="../../static/images/details-img3.png" mode="widthFix"></image>
				</view>
			</view>
		</view>
		
		<!-- 底部菜单按钮 -->
		<view class="xqbotomBar">
			<view class="left flexRowBetween" style=" width: 32%;">
				<view class="ite" style="width:50;" @click="prev">
					<image src="../../static/images/details=icon1.png" mode=""></image>
					<view>返回</view>
				</view>
				<view class="ite" style="width:50;" @click="showAddok">
					<image src="../../static/images/details=icon2.png" mode=""></image>
					<view>加入店铺</view>
				</view>
			</view>
			<view class="payBtn flexRowBetween" style="width: 68%;">
				<view class="item hei" @click="webSelf.$Router.navigateTo({route:{path:'/pages/confirmOrder/confirmOrder'}})">
					<view class="price">58.6</view>
					<view class="name">单独购买</view>
				</view>
				<view class="item yellow" @click="webSelf.$Router.navigateTo({route:{path:'/pages/confirmOrder/confirmOrder'}})">
					<view class="price">58.6</view>
					<view class="name">开团</view>
				</view>
			</view>
		</view>
		
		<!-- 拼团弹框 -->
		<view class="PinDanAlert" v-if="is_show">
			<view class="explain center">
				<view class="colseBtn" @click="goPinDan" style="left: auto;right: 10rpx;top: 20rpx;">×</view>
				<view class="tit">参与名称的拼单</view>
				<view><text class="red">还剩1个名额</text>。23:23:56后结束</view>
				<view class="flexCenter">
					<view class="photo">
						<image src="../../static/images/beestoenjoy-img1.png"></image>
					</view>
					<view class="photo noPic">
						<image src="../../static/images/who.png"></image>
					</view>
				</view>
				<view class="submitbtn">
					<button type="button" style="width: 100%; margin:20rpx 0;" @click="webSelf.$Router.navigateTo({route:{path:'/pages/confirmOrder/confirmOrder'}})">去拼单</button>
				</view>
			</view>
		</view>
		
		<!-- 加入店铺弹框 -->
		<view class="PinDanAlert" v-if="show_addok">
			<view class="explain center" style="background: none;">
			
				<view class="addShpPic">
					<image src="../../static/images/details-icon2.png" mode=""></image>
				</view>
				<view class="colseBtn" @click="showAddok" style="top: auto;bottom: -80rpx;border: 1rpx solid #fff; color: #fff;background: none;">×</view>
			</view>
			
			
			
		</view>
		
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				webSelf: this,
				showView: false,
				is_show:false,
				show_addok:false
			}
		},
		
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			goPinDan(){
				const self = this;
				self.is_show = !self.is_show
			},
			showAddok(){
				const self = this;
				self.show_addok = !self.show_addok
			},
			prev(){
				const self = this;
				self.$router.go(-1)
			},
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';

				self.$apis.orderGet(postData, callback);
			},
		},
	};
</script>

<style>
	@import "../../assets/style/page.css";
	page{padding-bottom: 140rpx;}
	
	.addShpPic{width: 420RPX; height: 390rpx; margin: 0 auto;}
.addShpPic image{width: 100%; height: 100%;}
</style>
