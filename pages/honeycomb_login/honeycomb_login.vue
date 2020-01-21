<template>
	<view v-if="showAll">
		<view class="loginBox">
			<view class="login-tit">账号密码登录</view>
			<view class="editBox">
				<view class="item">
					<input type="text" value="" placeholder="请输入手机号" v-model="submitData.login_name"/>
				</view>
				<view class="item">
					<input type="password" value="" placeholder="请输入密码" v-model="submitData.password" />
				</view>
			</view>
			<view class="submitbtn" style="margin-top: 200rpx;">
				<button class="hei" type="submit" style="width: 100%; margin-bottom: 20rpx;" 
				@click="submit">登录</button>
			</view>
			<view class="font13 color3" @click="Router.navigateTo({route:{path:'/pages/honeycomb_register/honeycomb_register'}})">没有账号，去注册</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				submitData:{
					login_name:'',
					password:''
				},
				showAll:false
			}
		},
		
		onLoad(options) {
			const self = this;
			if (uni.getStorageSync('merchantToken')) {
				uni.redirectTo({
					url: '/pages/honeycomb_center/honeycomb_center'
				})
			}else{
				self.showAll = true
			}
		},
		
		methods: {
			
			submit() {
				const self = this;
			
				const postData = {
					login_name: self.submitData.login_name,
					password:self.submitData.password
				};
				if (self.$Utils.checkComplete(self.submitData)) {
					
					const callback = (res) => {
						if (res.solely_code == 100000) {
							console.log(res);
							uni.setStorageSync('merchantToken', res.token);
							uni.setStorageSync('merchantInfo', res.info);
							uni.redirectTo({
								url: '/pages/honeycomb_center/honeycomb_center'
							}) 
						} else {
							self.$Utils.showToast(res.msg,'none')
						}
					}
					self.$apis.login(postData, callback);
				} else {
					self.$Utils.showToast('请补全登录信息', 'none')
				};
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

 
