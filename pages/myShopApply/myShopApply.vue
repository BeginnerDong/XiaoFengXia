<template>
	<view class="pub_editInfor">
		<view class="item">
			<view class="name">姓名</view>
			<view class="rr">
				<input type="text" placeholder="请输入姓名" v-model="submitData.name"/>
			</view>
		</view>
		<view class="item">
			<view class="name">电话</view>
			<view class="rr">
				<input type="number" maxlength="11" placeholder="请输入电话号码" v-model="submitData.phone"/>
			</view>
		</view>
		<view class="item">
			<view class="name">门店名称</view>
			<view class="rr">
				<input type="text" placeholder="请输入门店名称" v-model="submitData.shop_name"/>
			</view>
		</view>
		<view class="item">
			<view class="name">小区名称</view>
			<view class="rr">
				<input type="text" placeholder="请输入小区名称" v-model="submitData.village"/>
			</view>
		</view>
		<view class="item">
			<view class="name">小区地址</view>
			<view class="rr">
				<input type="text" placeholder="请输入小区地址" v-model="submitData.address"/>
			</view>
		</view>
		<view class="item">
			<view class="name">负责人</view>
			<view class="rr">
				<input type="text" placeholder="请输入负责人(部门维护人员)姓名" v-model="submitData.owner"/>
			</view>
		</view>
		
		
		<view class="submitbtn" style="margin-top: 200rpx;">
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
					name:'',
					phone:'',
					shop_name:'',
					village:'',
					address:'',
					owner:''
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
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				if(!wx.getStorageSync('user_info')||!wx.getStorageSync('user_info').headImgUrl){
				  postData.refreshToken = true;
				};
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				//postData.data.vip_time = Date.parse(new Date())/1000;
				if(self.mainData.level>3){
					postData.data.check_status = 1
				}
				const callback = (data) => {				
					if (data.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('提交成功', 'none');
						if(self.mainData.level<3){
							setTimeout(function() {
								self.Router.redirectTo({route:{path:'/pages/myShopApply_pay/myShopApply_pay'}})
							}, 800)
						}else{
							setTimeout(function() {
								self.Router.switchTab({route:{path:'/pages/index/index'}})
							}, 800)
						}	
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
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data[0];	
						self.submitData.name = self.mainData.name;
						self.submitData.phone = self.mainData.phone;
						self.submitData.shop_name = self.mainData.shop_name;
						self.submitData.village = self.mainData.village;
						self.submitData.address = self.mainData.address;
						self.submitData.owner = self.mainData.owner;
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

 
