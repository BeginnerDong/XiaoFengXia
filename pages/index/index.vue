<template>
	<view>
		<view class="index_topBj">
			<view class="adrsGps">
				<view class="dd">西安市雁塔区</view>
				<image class="icon" src="../../static/images/home-icon1.png" mode=""></image>
			</view>
			<view class="banner-box">
				<view class="banner radius10 oh">
					<swiper class="swiper-box" indicator-dots="indicatorDots" autoplay="autoplay" interval="interval" duration="duration" indicator-active-color="#434343">
						<block v-for="(item,index) in labelData" :key="index">
							<swiper-item class="swiper-item" @click="webSelf.$Router.navigateTo({route:{path:'/pages/simpleDetail/simpleDetail'}})">
								<image :src="item" class="slide-image" />
							</swiper-item>
						</block>
					</swiper>
				</view>
			</view>
		</view>
		<view class="f5bj pdlr4 inde_fivePic">
			<view class="flexRowBetween">
				<view class="item" style="width: 340rpx; height: 240rpx;" @click="webSelf.$Router.navigateTo({route:{path:'/pages/newPeople/newPeople'}})"> 
					<image src="../../static/images/home-img1.png" mode=""></image>
					<view class="textbox">
						<view class="tit">新人团</view>
						<view class="tex">点击进入</view>
						<view class="font12">&gt;&gt;&gt;</view>
					</view>
				</view>
				<view class="item" style="width: 330rpx; height: 240rpx;" @click="webSelf.$Router.navigateTo({route:{path:'/pages/myShopApply/myShopApply'}})">
					<image src="../../static/images/home-img2.png" mode=""></image>
					<view class="textbox">
						<view class="tit">我的店铺</view>
						<view class="tex">点击进入</view>
						<view class="font12">&gt;&gt;&gt;</view>
					</view>
				</view>
			</view>
			<view class="flexRowBetween threebox">
				<view class="item" @click="webSelf.$Router.navigateTo({route:{path:'/pages/goodsOpenDetail/goodsOpenDetail'}})">
					<image src="../../static/images/home-img3.png" mode=""></image>
					<view class="textbox">
						<view class="tit">中秋尝鲜</view>
						<view class="tex">吃出好生活</view>
					</view>
				</view>
				<view class="item" @click="webSelf.$Router.navigateTo({route:{path:'/pages/coupon/coupon'}})">
					<image src="../../static/images/home-img4.png" mode=""></image>
					<view class="textbox">
						<view class="tit">优惠券</view>
						<view class="tex">尽情畅买</view>
					</view>
				</view>
				<view class="item" @click="webSelf.$Router.switchTab({route:{path:'/pages/fengxiangba/fengxiangba'}})">
					<image src="../../static/images/home-img5.png" mode=""></image>
					<view class="textbox">
						<view class="tit">活动中心</view>
						<view class="tex">玩遍天南海北</view>
					</view>
				</view>
			</view>
			
		</view>
		
		<view class="f5H10"></view>
		<view class="infor-title flexRowBetween">
			<view class="tt">常规团</view>
			<view class="more" @click="webSelf.$Router.navigateTo({route:{path:'/pages/routine/routine'}})">更多&gt;</view>
		</view>
		<view class="pdlr4">
			<view style="padding-top: 30rpx;width: 100%; height: 260rpx;">
				<image style="width: 100%; height: 100%; display: block;border-radius: 10rpx;" src="../../static/images/home-img6.png"></image>
			</view>
			
			<view class="ind_proList">
				<view class="item flexRowBetween" @click="webSelf.$Router.navigateTo({route:{path:'/pages/goodsJoinDetail/goodsJoinDetail'}})">
					<view class="ll">
						<image src="../../static/images/home-img7.png" mode=""></image>
					</view>
					<view class="rr">
						<view class="avoidOverflow2 title">40枚正宗土鸡蛋农家散养新鲜农村野外放养笨鸡蛋草鸡蛋柴鸡蛋</view>
						<view class="lable">
							<view class="tt">5积分</view>
							<view class="tt">2人团</view>
						</view>
						<view class="money flexRowBetween">
							<view class="flex left">
								<view class="price">26.8</view>
								<view class="yuan price">36</view>
							</view>
							<view class="addbtn">参团</view>
						</view>
						
					</view>
				</view>
				<view class="item flexRowBetween" v-for="(item,index) in indProDate" :key="index" @click="webSelf.$Router.navigateTo({route:{path:'/pages/goodsOpenDetail/goodsOpenDetail'}})">
					<view class="ll">
						<image src="../../static/images/home-img7.png" mode=""></image>
					</view>
					<view class="rr">
						<view class="avoidOverflow2 title">40枚正宗土鸡蛋农家散养新鲜农村野外放养笨鸡蛋草鸡蛋柴鸡蛋</view>
						<view class="lable">
							<view class="tt">5积分</view>
							<view class="tt">2人团</view>
						</view>
						<view class="money flexRowBetween">
							<view class="flex left">
								<view class="price">26.8</view>
								<view class="yuan price">36</view>
							</view>
							<view class="addbtn">开团</view>
						</view>
						
					</view>
				</view>
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
				labelData: [
					"../../static/images/home-banner.png",
					"../../static/images/home-banner.png",
				],
				indProDate:[
					{},{},{},{}
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
	.index_topBj{background: #fff url(../../static/images/home-bBj.png) no-repeat 0 0/100% auto;padding-top:0}
	
</style>
