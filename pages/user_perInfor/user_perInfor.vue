<template>
	<view class="myRowBetween">
		<view class="item flexRowBetween">
			<view class="left">头像</view>
			<view class="right">
				<image style="width: 80rpx; height: 80rpx;" src="../../static/images/about-img1.png" mode=""></image>
			</view>
		</view>
		<view class="item flexRowBetween">
			<view class="left">昵称</view>
			<view class="right">快乐的猫</view>
		</view>
		<view class="item flexRowBetween">
			<view class="name">会员级别</view>
			<view class="rr">V5</view>
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
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';

				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data;
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getMainData');

				};
				self.$apis.orderGet(postData, callback);

			},

		},
	};
</script>
<style>
	page{background: #f5f5f5;}
</style>

 
