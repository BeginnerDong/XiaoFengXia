<template>
	<view class="pub_editInfor">
		<view class="item">
			<view class="name">地址</view>
			<view class="rr">
				<input type="text" v-model="submitData.address" />
			</view>
		</view>
		<view class="item">
			<view class="name">门店名称</view>
			<view class="rr">
				<input type="text" v-model="submitData.shop_name" placeholder="请输入门店名称"/>
			</view>
		</view>
		<view class="item">
			<view class="name">联系电话</view>
			<view class="rr">
				<input type="number" maxlength="11" v-model="submitData.phone"/>
			</view>
		</view>
		
		
		
		<view class="submitbtn" style="margin-top: 200rpx;">
			<button class="hei" type="submit" @click="Utils.stopMultiClick(submit)">确定</button>
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
					address:'',
					shop_name:'',
					phone:''
				}
			}
		},
		
		onLoad() {
			const self = this;
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
					
					self.userInfoUpdate();	
					
					
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},
			
			userInfoUpdate() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getMerchantToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('merchantInfo').user_no
				};
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				const callback = (data) => {				
					if (data.solely_code == 100000) {
						self.$Utils.showToast('修改成功', 'none');
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
				self.$apis.userInfoUpdate(postData, callback);
			},
			
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getMerchantToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('merchantInfo').user_no
				};
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data[0];
						self.submitData.address = self.mainData.address;
						self.submitData.shop_name = self.mainData.shop_name;
						self.submitData.phone = self.mainData.phone;
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userInfoGet(postData, callback);
			},

		},
	};
</script>

 
