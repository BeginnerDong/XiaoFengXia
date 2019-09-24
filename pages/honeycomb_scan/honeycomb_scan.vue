<template>
	<view>

		<view class="scanBox center">
			<view class="camera"></view>
			<view class="font13 text">将二维码放入框内，即可自动扫描</view>
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
	/* @import "../../assets/style/user.css"; */
	page{background: #7e7e7e;}
	.scanBox{ position: fixed; top: 50%;left: 0;width: 100%; transform: translateY(-60%);}
	.scanBox .camera{ width: 400rpx; height: 400rpx; border: 2rpx solid #F3D012; background: #fff;margin: 0 auto;}
	.scanBox .text{color: #fff; font-size: 26rpx; margin-top: 20rpx;}
</style>
