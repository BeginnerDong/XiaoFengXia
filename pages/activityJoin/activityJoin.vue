<template>
	<view class="pub_editInfor">
		<view class="pubColor center" style="padding-top: 30rpx;">您的会员等级V5</view>
		<view class="item">
			<view class="name">姓名</view>
			<view class="rr">
				<input type="text" placeholder="请输入姓名"/>
			</view>
		</view>
		<view class="item">
			<view class="name">联系方式</view>
			<view class="rr">
				<input type="number" maxlength="11" placeholder="请输入联系方式"/>
			</view>
		</view>
		<view class="item">
			<view class="name">验证码</view>
			<view class="rr pr flexRowBetween">
				<view class="" style="width:65%;">
					<input type="text" value="" placeholder="请输入验证码"/>
				</view>
				<view class="yzm">获取验证码</view>
			</view>
		</view>
		
		<view class="item">
			<view class="name">地址</view>
			<view class="rr">
				<input type="text" placeholder="请输地址"/>
			</view>
		</view>
		<view class="item">
			<view class="name">备注</view>
			<view class="rr">
				<textarea value="" placeholder="请输入备注信息"/>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 200rpx;">
			<button class="hei" type="submit">提交</button>
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
	.pub_editInfor .item .rr{text-align: right;}
</style>

 
