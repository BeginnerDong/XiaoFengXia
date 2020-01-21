<template>
	<view>
		
		<view class="tixianBox radius10">
			
		
			<view class="flexRowBetween bankmsg addBtn" v-if="userInfoData.bank">
				<view>到账银行卡</view>
				<view class="lft font12">
					{{userInfoData.bank}}
					<view class="color2 num">({{userInfoData.card_no}})</view>
				</view>
				<view @click="Router.navigateTo({route:{path:'/pages/myBankMsg/myBankMsg?type='+type}})">
					<image class="arrow arrowR" src="../../static/images/home-icon3.png" mode=""></image>
				</view>
			</view>
			
			<view class="flexRowBetween bankmsg addBtn" v-else>
				<view>到账银行卡</view>
				<view class="flexCenter" @click="Router.navigateTo({route:{path:'/pages/myBankMsg/myBankMsg?type='+type}})">
					<text class="color3 font13">去添加</text>
					<image class="arrow arrowR" src="../../static/images/home-icon3.png" mode=""></image>
				</view>
			</view>
		
			<view class="editMoney">
				<view class="tit">提现金额</view>
				<view class="input">
					<input type="number"  v-model="submitData.count" placeholder="请输入提现金额" placeholder-style="font-size:28rpx">
				</view>
				<view class="flexRowBetween font12 color3">
					<view>可提现金额{{userInfoData.balance}}元</view>
					<view style="color: #537DEB;" @click="allCount">全部提现</view>
				</view>
				<view class="submitbtn" style="margin-top: 100rpx;">
					<button class="hei" type="button" open-type="getUserInfo"  @getuserinfo="Utils.stopMultiClick(submit)">提现</button>
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
				Utils:this.$Utils,
				mainData:{},
				userInfoData:{},
				submitData:{
					count:''
				},
				type:''
			}
		},
		onLoad(options) {
			const self = this;
			self.type = options.type;
			console.log('self.type',self.type)
			//self.$Utils.loadAll(['getUserInfoData'], self);
		},
		
		onShow() {
			const self = this;
			self.$Utils.loadAll(['getUserInfoData'], self);
		},
		
		methods: {
			
			allCount(){
				const self = this;
				self.submitData.count = self.userInfoData.balance
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if(!self.userInfoData.bank){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请先绑定银行卡', 'none');
					return
				};
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('self.submitData',self.submitData)
				if (pass) {
					if(parseFloat(self.submitData.count)>parseFloat(self.userInfoData.balance)){
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('余额不足', 'none');
						return
					};
					if(parseFloat(self.submitData.count)<=0){
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('请输入正确的金额', 'none');
						return
					};
					
					const callback = (user, res) => {
						self.flowLogAdd();
					};
					self.$Utils.getAuthSetting(callback);	
					
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请输入提现金额', 'none')
				};
			},
			
			flowLogAdd() {
				const self = this;
				const postData = {
					searchItem:{}
				};
				if(!wx.getStorageSync('user_info')||!wx.getStorageSync('user_info').headImgUrl){
				  postData.refreshToken = true;
				};
				postData.data = {
					count:-self.submitData.count,
					thirdapp_id:2,
					status:0,
					trade_info:'提现',
					type:2,
				};		
				if(self.type=='merchant'){
					postData.tokenFuncName = 'getMerchantToken'
					postData.data.user_no = uni.getStorageSync('merchantInfo').user_no
				}else{
					postData.tokenFuncName = 'getProjectToken'
					postData.data.user_no = uni.getStorageSync('user_info').user_no
				};	
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('提交成功', 'none');
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 800)
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.flowLogAdd(postData, callback);
			},
			
			
			getUserInfoData() {
				const self = this;
				console.log('852369')
				const postData = {
					searchItem:{}
				};
				if(self.type=='merchant'){
					postData.tokenFuncName = 'getMerchantToken'
					postData.searchItem.user_no = uni.getStorageSync('merchantInfo').user_no
				}else{
					postData.tokenFuncName = 'getProjectToken'
					postData.searchItem.user_no = uni.getStorageSync('user_info').user_no
				};		
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.userInfoData = res.info.data[0]
						self.userInfoData.card_no = self.userInfoData.card_no .substr(self.userInfoData.card_no.length-4)
					} else {
						self.$Utils.showToast(res.msg, 'none');
					};
					self.$Utils.finishFunc('getUserInfoData');
			
				};
				self.$apis.userInfoGet(postData, callback);
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
