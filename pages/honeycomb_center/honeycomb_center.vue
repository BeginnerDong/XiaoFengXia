<template>
	<view>

		<view class="userHead">
			<view class="left flex">
				<view class="photo">
					<image src="../../static/images/about-img1.png" mode=""></image>
				</view>
				<view class="infor" >
					<view class="font14">赶往陌生的下一站</view>
				</view>
			</view>
		</view>
		
		<view class="f5H10"></view>
		<view class="XlineNav">
			<view class="info" @click="Router.navigateTo({route:{path:'/pages/honeycomb_scan/honeycomb_scan'}})">
				<view class="ilblock listimg">
					<image src="../../static/images/merchants-icon1.png"></image>
				</view>
				<view class="ilblock">扫一扫</view>
				<image class="arrowR" src="../../static/images/home-icon3.png" ></image>
			</view>
			<view class="info" @click="Router.navigateTo({route:{path:'/pages/honeycomb_shopMsg/honeycomb_shopMsg'}})">
				<view class="ilblock listimg">
					<image src="../../static/images/merchants-icon2.png"></image>
				</view>
				<view class="ilblock">门店管理</view>
				<image class="arrowR" src="../../static/images/home-icon3.png" ></image>
			</view>
			<view class="info" @click="Router.navigateTo({route:{path:'/pages/honeycomb_writeOff/honeycomb_writeOff'}})">
				<view class="ilblock listimg">
					<image src="../../static/images/merchants-icon4.png"></image>
				</view>
				<view class="ilblock">核销订单</view>
				<image class="arrowR" src="../../static/images/home-icon3.png" ></image>
			</view>
			<view class="info" @click="Router.navigateTo({route:{path:'/pages/myYongJin/myYongJin'}})">
				<view class="ilblock listimg">
					<image src="../../static/images/merchants-icon3.png"></image>
				</view>
				<view class="ilblock">我的佣金</view>
				<image class="arrowR" src="../../static/images/home-icon3.png" ></image>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
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
