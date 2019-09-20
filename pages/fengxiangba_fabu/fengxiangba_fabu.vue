<template>
	<view>
		
		<view class="pub_editColumn">
			<view class="item">
				<input type="text" value="" placeholder="给你的文章写个标题吧~" />
			</view>
			<view class="item">
				<textarea value="" placeholder=""  placeholder="发点你想说的~" />
			</view>
			<view class="item">
				<view class="upPic">
					<image src="../../static/images/release-icon2.png" mode=""></image>
				</view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 200rpx;">
			<button class="hei" type="submit">确认发布</button>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				webSelf: this,
				showView: false,
				wx_info:{}
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
	@import "../../assets/style/page.css";

	page {padding-bottom: 60rpx;}
	
</style>
