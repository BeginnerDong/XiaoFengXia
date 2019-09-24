<template>
	<view>
		<view class="detailxqBan" style="height: 400rpx;">
			<image src="../../static/images/shenqing-img1.png" mode=""></image>
		</view>
		
		<view class="pub_editInfor">
			<view class="item">
				<view class="name">姓名：</view>
				<view class="rr">
					<input type="text" placeholder="请输入姓名"/>
				</view>
			</view>
			<view class="item">
				<view class="name">登录手机：</view>
				<view class="rr">
					<input type="number" maxlength="11" placeholder="请输入登录手机号"/>
				</view>
			</view>
			<view class="item">
				<view class="name">登录密码：</view>
				<view class="rr">
					<input type="password" placeholder="请输入登录密码(6-16位)"/>
				</view>
			</view>
			<view class="item">
				<view class="name">详细地址：</view>
				<view class="rr">
					<input type="text" placeholder="请输入小区详细地址"/>
				</view>
			</view>
			
			
			<view class="submitbtn" style="margin-top: 200rpx;">
				<button class="hei" type="submit" @click="Router.navigateTo({route:{path:'/pages/myShopApply_pay/myShopApply_pay'}})">确定</button>
			</view>
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
	@import "../../assets/style/page.css";
</style>

 
