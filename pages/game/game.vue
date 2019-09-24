<template>
	<view>
		<view class="game_indexList pdlr4">
			<view class="item radius10" style="background: #f84a49;" @click="Router.navigateTo({route:{path:'/pages/game_turntable/game_turntable'}})">
				<image src="../../static/images/youxizhongxin-img1.png" mode=""></image>
			</view>
			<view class="item radius10" style="background: #9838d0;" @click="Router.navigateTo({route:{path:'/pages/game_bargain/game_bargain'}})">
				<image src="../../static/images/youxizhongxin-img2.png" mode=""></image>
			</view>
		</view>
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				showView: false,
				Router:this.$Router,
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
	.game_indexList .item{padding: 20rpx 30rpx;box-sizing: border-box;overflow: hidden; margin-top: 30rpx;}
	.game_indexList .item image{width: 445rpx; height: 301rpx; display: block; margin: 0 auto;}
	
</style>
