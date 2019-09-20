<template>
	<view class="pdlr4">
		
		<view class="tit" style=""  @click="webSelf.$Router.navigateTo({route:{path:'/pages/game_turntable_rule/game_turntable_rule'}})">
			<view class="name">活动规则</view>
		</view>
		
		<view class="turntable">
			<image class="pic" src="../../static/images/game-icon2.png" mode=""></image>
		</view>
		
		<view class="submitbtn">
			<button class="hei" type="button">你还有5次机会</button>
		</view>
		<view class="submitbtn" style="margin-top: 50rpx;">
			<button type="button"  @click="webSelf.$Router.navigateTo({route:{path:'/pages/game_turntable_winRecord/game_turntable_winRecord'}})">中奖纪录</button>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				webSelf: this,
				showView: false
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
	page {padding-bottom: 60rpx;}
	
	.tit{ display: flex; justify-content: flex-end;padding: ;}
	.tit .name{font-size: 24rpx; padding: 20rpx;}
	.turntable{width: 580rpx; height: 580rpx; margin: 40rpx auto 60rpx auto;}
	.turntable .pic{width: 100%; height: 100%; display: block;}
</style>
