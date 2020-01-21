<template>
	<view class="pub_editInfor">
		<view class="item">
			<view class="name">持卡人:</view>
			<view class="rr">
				<input type="text" placeholder="请输入持卡人的姓名" v-model="submitData.card_name"/>
			</view>
		</view>
		<view class="item">
			<view class="name">手机号：</view>
			<view class="rr">
				<input type="number" maxlength="11" placeholder="请输入您的银行卡绑定的手机号" v-model="submitData.card_phone"/>
			</view>
		</view>
		<view class="item">
			<view class="name">开户行：</view>
			<view class="rr">
				<input type="text" placeholder="请输入您的银行卡的开户行" v-model="submitData.bank"/>
			</view>
		</view>
		<view class="item">
			<view class="name">银行卡号：</view>
			<view class="rr">
				<input type="number" placeholder="请输入您的银行卡号" v-model="submitData.card_no"/>
			</view>
		</view>
		
		
		<view class="submitbtn" style="margin-top: 200rpx;" >
			<button class="hei" type="submit" open-type="getUserInfo"  @getuserinfo="Utils.stopMultiClick(submit)">确定</button>
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
				submitData:{
					bank:'',
					card_name:'',
					card_no:'',
					card_phone:''
				}
			}
		},
		onLoad(options) {
			const self = this;
			self.type = options.type;
			console.log('self.type',self.type)
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			submit() {
				const self = this;
				
				uni.setStorageSync('canClick', false);
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('pass', pass);
				console.log('self.submitData',self.submitData)
				if (pass) {	
					const callback = (user, res) => {
						self.userInfoUpdate();	
					};
					self.$Utils.getAuthSetting(callback);
					
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},
			
			userInfoUpdate() {
				const self = this;
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
				if(!wx.getStorageSync('user_info')||!wx.getStorageSync('user_info').headImgUrl){
				  postData.refreshToken = true;
				};
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				//postData.data.vip_time = Date.parse(new Date())/1000;
				const callback = (data) => {				
					if (data.solely_code == 100000) {
						self.$Utils.showToast('保存成功', 'none');
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 800);
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.userInfoUpdate(postData, callback);
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					
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
						self.mainData = res.info.data[0];	
						self.submitData.bank = self.mainData.bank;
						self.submitData.card_no = self.mainData.card_no;
						self.submitData.card_name = self.mainData.card_name;
						self.submitData.card_phone = self.mainData.card_phone;
					} else {
						self.$Utils.showToast(res.msg, 'none');
					};
					self.$Utils.finishFunc('getMainData');
			
				};
				self.$apis.userInfoGet(postData, callback);
			},
			

		},
	};
</script>

 
