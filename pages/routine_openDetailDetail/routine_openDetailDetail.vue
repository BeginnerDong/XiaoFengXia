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
				<view class="item hei" @click="Router.navigateTo({route:{path:'/pages/confirmOrder/confirmOrder'}})">
					<view class="price">58.6</view>
					<view class="name">单独购买</view>
				</view>
				<view class="item yellow" @click="Router.navigateTo({route:{path:'/pages/confirmOrder/confirmOrder'}})">
					<view class="price">58.6</view>
					<view class="name">开团</view>
				</view>
			</view>
		</view>
		
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false
			}
		},
		
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
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
	@import "../../assets/style/car.css";
	page{padding-bottom: 140rpx;}
	.payBtn .item{ width: 33.3%;}
	
	.showSel{width: 100%;height: 100%;background: rgba(0, 0, 0, 0.5);position: fixed;top: 0;right: 0;bottom: 0;left: 0;z-index: 10000;}
	.showSelCont {padding:40rpx 4%;background: #fff;}
	.showSelCont.fix{position: fixed;bottom: 0;left: 0;box-sizing: border-box;width: 100%;border-radius: 20rpx 20rpx 0 0;}
	.yy-title {font-size: 28rpx;}
	.yyDate-lis {max-height: 270rpx; overflow-y: auto;}
	.yyDate-lis view {width: 130rpx;height: 56rpx;font-size: 26rpx;color: #666;text-align: center;line-height: 56rpx;background: #f5f5f5;margin: 0 20rpx 30rpx 20rpx;}
	.yyDate-lis view.on{background: #F3D012; color: #333;}
	.showSel .numBox view {background: #f5f5f5;}
	.detal_guige{padding: 20rpx 4%;}
	.detal_guige .ll{width: 90%; display: flex; justify-content: flex-start;align-items: center;}
	.detal_guige .rr{width: 10%;display: flex;justify-content: flex-end;}
	.detal_guige .yyDate-lis view{margin:0 0 0 20rpx;}
</style>
