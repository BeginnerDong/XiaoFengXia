<template>
	<view>
		<view class="ewma">
			<image :src="mainData.qrcode" mode=""></image>
		</view>
		
	</view>

</template>

<script>
	export default {
		data() {
			return {
				Router: this.$Router,
				mainData: {}
			}
		},

		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},

		methods: {

			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id: self.id
				}
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data[0];
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
		}
	}
</script>

<style>
	.ewma{padding-top:300rpx}
	.ewma image{width: 360rpx; height:360rpx; display: block;margin: 0 auto;}
</style>
