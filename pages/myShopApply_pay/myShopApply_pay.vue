<template>
	<view class="">
		
		<view class="shopPay">
			<view class="item boxShaow" @click="choose(index)" v-for="(item,index) in choosePay" :key="index">
				<view class="icon">
					<image :src="item.iconUrl" mode=""></image>
				</view>
				<view class="money">{{item.level}}<text class="price">{{item.price}}</text></view>
				<view class="choose">
					<image :src="curr==index?'../../static/images/pay-icon5.png':'../../static/images/pay-icon6.png'" mode=""></image>
				</view>
			</view>
			
		</view>
		<view class="submitbtn" style="margin-top: 160rpx;">
			<button class="hei" type="submit" @click="Router.navigateTo({route:{path:'/pages/myShop/myShop'}})">支付</button>
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
				wx_info:{},
				curr:1,
				choosePay:[
					{
						iconUrl:'../../static/images/pay-icon1.png',
						level:'V3',
						price:'56'
					},
					{
						iconUrl:'../../static/images/pay-icon2.png',
						level:'V4',
						price:'66'
					},
					{
						iconUrl:'../../static/images/pay-icon3.png',
						level:'V5',
						price:'76'
					},
					{
						iconUrl:'../../static/images/pay-icon4.png',
						level:'V6',
						price:'77'
					}
				]
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			choose(index){
				const self = this;
				self.curr = index
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


 
