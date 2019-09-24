<template>
	<view>
		<view class="ewma">
			<image src="../../static/images/hexiaoma.png" mode=""></image>
		</view>
		
	</view>

</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false
			}
		},

		methods: {
			getMainData() {
				const self = this;
				self.$apis.userGet(postData, callback);
			}
		}
	}
</script>

<style>
	.ewma{padding-top:300rpx}
	.ewma image{width: 360rpx; height:360rpx; display: block;margin: 0 auto;}
</style>
