<template>
	<view>
		<view class="posterPic">
			<view class="back_icon" @click="prev"><image src="../../static/images/left-arrow.png" mode=""></image></view>
			<image class="img" src="../../static/images/posters-img1.png" mode=""></image>
			<image class="ewm" src="../../static/images/posters-img2.png" mode=""></image>
		</view>
		
		
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				webSelf: this,
				showView: false,
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
	.back_icon{position: absolute; top: 60rpx;left: 4%; z-index: 10;}
	.back_icon image{width: 20rpx; height: 32rpx; display: block;}
	.posterPic{ position: fixed; top: 0;right: 0; left: 0;bottom: 0; overflow: hidden; z-index: 10;}
	.posterPic .img{ width: 100%; height: 100%; display: block;}
	.posterPic .ewm{width: 150rpx; height:150rpx; display: block; position: absolute; bottom: 70rpx;left: 40rpx;}
	uni-page-head .uni-page-head{background: none!important;}
</style>
