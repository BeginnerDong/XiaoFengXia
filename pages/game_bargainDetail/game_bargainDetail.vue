<template>
	<view>
		
		<view class="detailxqBan">
			<image src="../../static/images/details-img1.png" mode=""></image>
		</view>
		
		<view class="detail_one pr">
			<view class="">
				<view class="price font14">56.5</view>
			</view>
			<view class="flexRowAround shareBtn" style="justify-content: flex-end;">
				<image src="../../static/images/beestoenjoy-icon1.png" mode=""></image>
				<view class="" style="">分享</view>
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
			<view class="left flexRowBetween">
				<view class="ite" @click="prev">
					<image src="../../static/images/details=icon1.png" mode=""></image>
					<view>返回</view>
				</view>
			</view>
			<view class="payBtn flexRowBetween">
				<view class="item hei" style="width: 100%;" @click="webSelf.$Router.navigateTo({route:{path:'/pages/confirmOrder/confirmOrder'}})">
					<view class="price">586</view>
				</view>
			</view>
		</view>
		
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				showView: false,
				Router:this.$Router
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
	.payBtn .hei .price::before{content: "总计：";}
	.shareBtn{font-size: 24rpx;padding: 0 20rpx;border-radius:30rpx 0 0 30rpx; line-height:46rpx;background: #eee; color: #666; margin-left: 6rpx;position: absolute; right: 0;top: 20rpx;}
	.shareBtn image{width: 26rpx; height: 29rpx;display: block;margin-right: 6rpx;}
</style>
