<template>
	<view>
		<view class="index_topBj pdlr4">
			<view class="banner-box" style="padding-top: 30rpx 0;">
				<view class="banner radius10 oh">
					<swiper class="swiper-box" indicator-dots="indicatorDots" autoplay="autoplay" interval="interval" duration="duration" indicator-active-color="#434343">
						<block v-for="(item,index) in labelData" :key="index">
							<swiper-item class="swiper-item">
								<image :src="item" class="slide-image" />
							</swiper-item>
						</block>
					</swiper>
				</view>
			</view>
		</view>
		
		<view class="f5H10"></view>
		<view class="ind_cont5">
			<view class="flexRowBetween">
				<view class="item" @click="webSelf.$Router.navigateTo({route:{path:'/pages/coupon/coupon'}})">
					<image src="../../static/images/mall-icon1.png" mode=""></image>
					<view class="tit">优惠券</view>
				</view>
				<view class="item"  @click="webSelf.$Router.navigateTo({route:{path:'/pages/game/game'}})">
					<image src="../../static/images/mall-icon2.png" mode=""></image>
					<view class="tit">游戏</view>
				</view>
			</view>
		</view>
		
		<view class="f5H10"></view>
		<view class="infor-title flexRowBetween">
			<view class="tt">精选推荐</view>
		</view>
		<view class="proLis flexRowBetween">
			<view class="item-lis" v-for="(item,index) in produtList" :key="index" @click="webSelf.$Router.navigateTo({route:{path:'/pages/jiFenShopDetail/jiFenShopDetail'}})">
				<image class="img" src="../../static/images/beestoenjoy.png" alt="" />
				<view class="tit avoidOverflow">名称名称名称名称名称</view>
				<view class="price">23.6<text>/积分:233</text></view>
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
				score:'',
				wx_info:{},
				scrollTop: 100,
				index: 0,
				labelData:[
					"../../static/images/home-banner.png",
					"../../static/images/home-banner.png",
				],
				produtList:[
					{},{},{},{},{},{}
				]
			}
		},
		
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
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

	page {
		padding-bottom: 60rpx;
	}
	.ind_cont5{padding: 30rpx 4%;}
	.ind_cont5 .item{width: 330rpx;height: 200rpx;overflow: hidden;position: relative;border-radius: 8rpx;}
	.ind_cont5 .item image{width: 100%;height: 100%;}
	.ind_cont5 .item .tit{position: absolute;top: 36rpx;left: 26rpx;color: #fff;font-size:36rpx;}
	
	.proLis{padding: 20rpx 4%;flex-wrap: wrap;}
	.proLis .item-lis{ width: 330rpx; overflow: hidden;background: #fff;margin: 15rpx 0; position: relative;border-radius: 10rpx 10rpx 0 0;}
	.proLis .item-lis .img{width: 100%;height: 330rpx;display: block;margin: 0 auto;}
	.proLis .item-lis .tit{ font-size: 26rpx; padding: 20rpx 4% 10rpx 4%; line-height: 32rpx;}
	.proLis .item-lis .price{font-size: 28rpx;padding: 0 4%; color: #ff3b3b;}
	.proLis .item-lis .price:before{content: "￥"; font-size: 26rpx;margin-right:4rpx;}
</style>
