<template>
	<view class="">
		
		<view class="shopPay">
			<view class="item boxShaow" @click="choose('1')">
				<view class="icon">
					<image src="../../static/images/pay-icon1.png" mode=""></image>
				</view>
				<view class="money">v3<text class="price">56</text></view>
				<view class="choose">
					<image :src="index==1?'../../static/images/pay-icon5.png':'../../static/images/pay-icon6.png'" mode=""></image>
				</view>
			</view>
			<view class="item boxShaow"  @click="choose('2')">
				<view class="icon">
					<image src="../../static/images/pay-icon3.png" mode=""></image>
				</view>
				<view class="money">v4<text class="price">66</text></view>
				<view class="choose">
					<image :src="index==2?'../../static/images/pay-icon5.png':'../../static/images/pay-icon6.png'" mode=""></image>
				</view>
			</view>
			<view class="item boxShaow" @click="choose('3')">
				<view class="icon">
					<image src="../../static/images/pay-icon2.png" mode=""></image>
				</view>
				<view class="money">v5<text class="price">76</text></view>
				<view class="choose">
					<image :src="index==3?'../../static/images/pay-icon5.png':'../../static/images/pay-icon6.png'" mode=""></image>
				</view>
			</view>
			<view class="item boxShaow" @click="choose('4')">
				<view class="icon">
					<image src="../../static/images/pay-icon4.png" mode=""></image>
				</view>
				<view class="money">v6<text class="price">88</text></view>
				<view class="choose">
					<image :src="index==4?'../../static/images/pay-icon5.png':'../../static/images/pay-icon6.png'" mode=""></image>
				</view>
			</view>
		</view>
		<view class="submitbtn" style="margin-top: 160rpx;">
			<button class="hei" type="submit" @click="webSelf.$Router.navigateTo({route:{path:'/pages/myShop/myShop'}})">支付</button>
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
				index:1
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			choose(index){
				const self = this;
				if(index!=self.index){
					self.index = index
				}
			},
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';

			},

		},
	};
</script>

<style>
	.shopPay{padding: 40rpx 4%; display: flex;justify-content: space-between; flex-wrap: wrap;}
	.shopPay .item{ width: 48%;padding: 40rpx 30rpx;box-sizing: border-box;text-align: center;border-radius: 10rpx; margin-bottom: 30rpx;}
	.shopPay .item .icon{ width: 110rpx; height: 110rpx; margin: 0 auto;}
	.shopPay .item .icon image{width: 100%; height: 100%;}
	.shopPay .item .money{margin: 30rpx auto;}
	.shopPay .item .choose image{width: 40rpx; height: 40rpx; margin: 0 auto; display: block;}
</style>


 
