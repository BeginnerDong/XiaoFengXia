<template>
	<view class="pub_editInfor">
		<view class="pubColor center" style="padding-top: 30rpx;">您的会员等级V3</view>
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
			<view class="name">登录密码</view>
			<view class="rr">
				<input type="password" placeholder="请输登录密码(6-16位)"/>
			</view>
		</view>
		<view class="item">
			<view class="name">电话：</view>
			<view class="rr">
				<input type="number" placeholder="请输入电话号码"/>
			</view>
		</view>
		<view class="item">
			<view class="name">门店名称</view>
			<view class="rr">
				<input type="text" placeholder="请输入门店名称"/>
			</view>
		</view>
		<view class="item">
			<view class="name">小区名称</view>
			<view class="rr">
				<input type="text" placeholder="请输入小区名称"/>
			</view>
		</view>
		<view class="item">
			<view class="name">小区地址</view>
			<view class="rr">
				<input type="text" placeholder="请输入小区地址"/>
			</view>
		</view>
		
		<view class="item">
			<view class="name">负责人</view>
			<view class="rr">
				<input type="text" placeholder="请输入负责人(部门维护人员)姓名"/>
			</view>
		</view>
		
		
		<view class="submitbtn" style="margin-top: 200rpx;">
			<button class="hei" type="submit" @click="webSelf.$Router.navigateTo({route:{path:'/pages/honeycomb_login/honeycomb_login'}})">确定</button>
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

 
