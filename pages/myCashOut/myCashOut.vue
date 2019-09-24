<template>
	<view>
		
		<view class="tixianBox radius10">
			<view class="flexRowBetween bankmsg addBtn" >
				<view>到账银行卡</view>
				<view class="flexCenter" @click="Router.navigateTo({route:{path:'/pages/myBankMsg/myBankMsg'}})">
					<text class="color3 font13">去添加</text>
					<image class="arrow arrowR" src="../../static/images/home-icon3.png" mode=""></image>
				</view>
			</view>
		
			<view class="flexRowBetween bankmsg addBtn">
				<view>到账银行卡</view>
				<view class="lft font12">
					中国银行
					<view class="color2 num">(2356)</view>
				</view>
				<view @click="Router.navigateTo({route:{path:'/pages/myBankMsg/myBankMsg'}})">
					<image class="arrow arrowR" src="../../static/images/home-icon3.png" mode=""></image>
				</view>
			</view>
		
			<view class="editMoney">
				<view class="tit">提现金额</view>
				<view class="input">
					<input type="number" value="" placeholder="请输入提现金额" placeholder-style="font-size:28rpx">
				</view>
				<view class="flexRowBetween font12 color3">
					<view>可提现金额200元</view>
					<view style="color: #537DEB;">全部提现</view>
				</view>
				<view class="submitbtn" style="margin-top: 100rpx;">
					<button class="hei" type="button">提现</button>
				</view>
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
				self.$apis.orderGet(postData, callback);

			},

		},
	};
</script>

<style>
	/* @import "../../assets/style/common.css"; */
	page{ background: #f5f5f5;}
	
	.tixianBox{margin:100rpx 4%;background: #fff; }
	
	.bankmsg .lft{ display: flex; width: 50%; justify-content: flex-start; align-items: center; font-size: 26rpx;color: #999;}
	.bankmsg .lft .num{ margin-left: 20rpx; font-size: 26rpx;}
	
	.addBtn{border-radius: 10rpx; height: 100rpx; font-size: 30rpx;padding: 0 30rpx;}
	.addBtn .arrow{ margin-left: 20rpx;}
	
	.editMoney{padding: 30rpx;}
	.editMoney .tit{ font-size: 28rpx; line-height: 80rpx;}
	.editMoney .input{ display: flex; justify-content: flex-start; line-height: 100rpx; margin-bottom: 30rpx;border-bottom: 2rpx solid #e7e7e7;}
	.editMoney .input input{ font-size: 72rpx; line-height: 100rpx; display: block; width: 80%; height: 100rpx; box-sizing: border-box; }
	.editMoney .input input[placeholder]{font-size: 32rpx;}
	.editMoney .input::before{content: "￥"; font-size: 40rpx; margin-right: 10rpx;}
</style> 
