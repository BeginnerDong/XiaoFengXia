<template>
	<view class="pub_editInfor">
		<view class="pubColor center" style="padding-top: 30rpx;">您的会员等级V{{mainData.level}}</view>
		<view class="item">
			<view class="name">姓名：</view>
			<view class="rr">
				<input type="text" placeholder="请输入姓名" v-model="submitData.name"/>
			</view>
		</view>
		<view class="item">
			<view class="name">登录手机：</view>
			<view class="rr">
				<input type="number" maxlength="11" placeholder="请输入登录手机号" v-model="submitData.login_name"/>
			</view>
		</view>
		<view class="item">
			<view class="name">登录密码</view>
			<view class="rr">
				<input type="password" placeholder="请输登录密码(6-16位)" v-model="submitData.password"/>
			</view>
		</view>
		<view class="item">
			<view class="name">电话：</view>
			<view class="rr">
				<input type="number" placeholder="请输入电话号码" v-model="submitData.phone"/>
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
				Utils:this.$Utils,
				Router:this.$Router,
				mainData:{},
				submitData:{
					name:'',
					login_name:'',
					password:'',
					phone:'',
					shop_name:'',
					village:'',
					address:'',
					owner:'',
					thirdapp_id:2
				}
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];	
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('pass', pass);
				console.log('self.submitData',self.submitData)
				if (pass) {
						const callback = (user, res) => {
							self.register();
						};
						self.$Utils.getAuthSetting(callback);
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},
			
			register() {
				const self = this;
				const postData = {};
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('注册成功', 'none');
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
				self.$apis.register(postData, callback);
			},

		},
	};
</script>

 
