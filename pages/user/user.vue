<template>
	<view>

		<view class="userHead flexRowBetween"  @click="webSelf.$Router.navigateTo({route:{path:'/pages/user_perInfor/user_perInfor'}})">
			<view class="left flex">
				<view class="photo">
					<image src="../../static/images/about-img1.png" mode=""></image>
				</view>
				<view class="infor" >
					<view class="font14">赶往陌生的下一站</view>
					<view class="lable">会员V5</view>
				</view>
			</view>
			<view class="arrow">
				<image class="arrowR" src="../../static/images/home-icon3.png" ></image>
			</view>
		</view>
		
		<view class="f5H10"></view>
		<view class="XlineNav">
			<view class="info" @click="webSelf.$Router.navigateTo({route:{path:'/pages/myOrder/myOrder'}})">
				<view class="ilblock listimg">
					<image src="../../static/images/about-icon1.png"></image>
				</view>
				<view class="ilblock">我的订单</view>
				<image class="arrowR" src="../../static/images/home-icon3.png" ></image>
			</view>
			<view class="info" @click="webSelf.$Router.navigateTo({route:{path:'/pages/myIntegral/myIntegral'}})">
				<view class="ilblock listimg">
					<image src="../../static/images/about-icon2.png"></image>
				</view>
				<view class="ilblock">我的积分</view>
				<image class="arrowR" src="../../static/images/home-icon3.png" ></image>
			</view>
			<view class="info" @click="webSelf.$Router.navigateTo({route:{path:'/pages/myCoupon/myCoupon'}})">
				<view class="ilblock listimg">
					<image src="../../static/images/about-icon3.png"></image>
				</view>
				<view class="ilblock">我的优惠券</view>
				<image class="arrowR" src="../../static/images/home-icon3.png" ></image>
			</view>
			<view class="info" @click="webSelf.$Router.navigateTo({route:{path:'/pages/myYongJin/myYongJin'}})">
				<view class="ilblock listimg">
					<image src="../../static/images/about-icon4.png"></image>
				</view>
				<view class="ilblock">我的佣金</view>
				<image class="arrowR" src="../../static/images/home-icon3.png" ></image>
			</view>
			<view class="info" @click="webSelf.$Router.navigateTo({route:{path:'/pages/myAddress/myAddress'}})">
				<view class="ilblock listimg">
					<image src="../../static/images/about-icon5.png"></image>
				</view>
				<view class="ilblock">我的地址</view>
				<image class="arrowR" src="../../static/images/home-icon3.png" ></image>
			</view>
			<view class="info" @click="webSelf.$Router.navigateTo({route:{path:'/pages/myCommunityCenter/myCommunityCenter'}})">
				<view class="ilblock listimg">
					<image src="../../static/images/about-icon6.png"></image>
				</view>
				<view class="ilblock">社区中心</view>
				<image class="arrowR" src="../../static/images/home-icon3.png" ></image>
			</view>
			
			<view class="f5H10"></view>
			<view class="info" @click="webSelf.$Router.navigateTo({route:{path:'/pages/honeycomb_login/honeycomb_login'}})">
				<view class="ilblock listimg">
					<image src="../../static/images/about-icon7.png"></image>
				</view>
				<view class="ilblock">蜂窝中心</view>
				<image class="arrowR" src="../../static/images/home-icon3.png" ></image>
			</view>
			<view class="info" @click="webSelf.$Router.navigateTo({route:{path:'/pages/myBeeman_register/myBeeman_register'}})">
				<view class="ilblock listimg">
					<image src="../../static/images/about-icon8.png"></image>
				</view>
				<view class="ilblock">蜂客申请入口</view>
				<image class="arrowR" src="../../static/images/home-icon3.png" ></image>
			</view>
			
			<view class="f5H10"></view>
			<view class="info">
				<view class="ilblock listimg">
					<image src="../../static/images/about-icon9.png"></image>
				</view>
				<view class="ilblock">在线客服</view>
				<image class="arrowR" src="../../static/images/home-icon3.png" ></image>
			</view>
		</view>
		<view style="height: 40rpx;width: 100%;background: #f5f5f5;"></view>

		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				webSelf: this,
				showView: false,
				score:'',
				wx_info:{}
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {


		},
	};
</script>

<style>
	@import "../../assets/style/user.css";

</style>
