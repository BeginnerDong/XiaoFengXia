<template>
	<view>
		<view class="loginBox">
			<view class="login-tit">账号密码登录</view>
			<view class="editBox">
				<view class="item">
					<input type="text" value="" placeholder="请输入手机号" />
				</view>
				<view class="item">
					<input type="text" value="" placeholder="请输入密码" />
				</view>
			</view>
			<view class="submitbtn" style="margin-top: 200rpx;">
				<button class="hei" type="submit" style="width: 100%; margin-bottom: 20rpx;" @click="webSelf.$Router.navigateTo({route:{path:'/pages/honeycomb_center/honeycomb_center'}})">登录</button>
			</view>
			<view class="font13 color3" @click="webSelf.$Router.navigateTo({route:{path:'/pages/honeycomb_register/honeycomb_register'}})">没有账号，去注册</view>
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
				self.$apis.orderGet(postData, callback);
			},

		},
	};
</script>
<style>
	@import "../../assets/style/page.css";
	.loginBox{padding:0 10%;}
	.login-tit{ font-size:44rpx; padding: 100rpx 0 140rpx 0;}
	.loginBox .editBox .item{padding:0 10rpx 0;border-bottom: 2rpx solid #e7e7e7; margin-bottom: 60rpx;}
	.loginBox .editBox input{width: 100%; display: block; line-height: 60rpx; height: 60rpx;}
</style>

 
