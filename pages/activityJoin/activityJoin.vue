<template>
	<view class="pub_editInfor">
		<view class="pubColor center" style="padding-top: 30rpx;">您的会员等级V{{mainData.level}}</view>
		<view class="item">
			<view class="name">姓名</view>
			<view class="rr">
				<input type="text" placeholder="请输入姓名" v-model="submitData.title"/>
			</view>
		</view>
		<view class="item">
			<view class="name">联系方式</view>
			<view class="rr">
				<input type="number" maxlength="11" placeholder="请输入联系方式" v-model="submitData.phone"/>
			</view>
		</view>
		<view class="item">
			<view class="name">验证码</view>
			<view class="rr pr flexRowBetween">
				<view class="" style="width:65%;">
					<input type="text" value="" placeholder="请输入验证码" v-model="submitData.code"/>
				</view>
				<view class="yzm"  @click="sendCode" v-if="!hasSend">{{text}}</view>
				<view class="yzm"   v-else>{{text}}</view>
			</view>
		</view>
		
		<view class="item">
			<view class="name">地址</view>
			<view class="rr">
				<input type="text" placeholder="请输入地址" v-model="submitData.description"/>
			</view>
		</view>
		<view class="item">
			<view class="name">备注</view>
			<view class="rr">
				<textarea value="" placeholder="请输入备注信息" v-model="submitData.content"/>
			</view>
		</view>
		
			<view class="submitbtn" style="margin-top: 200rpx;">
				<button class="hei" type="submit" open-type="getUserInfo"  @getuserinfo="Utils.stopMultiClick(submit)">提交</button>
			</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				submitData:{
					title:'',
					phone:'',
					description:'',
					content:'',
					relation_table:'article',
					relation_id:'',
					type:3,
					code:''
				},
				mainData:{},
				currentTime:61,
				text:'获取验证码',
				hasSend:false,
			}
		},
		
		onLoad(options) {
			const self = this;
			uni.setStorageSync('canClick', true);
			self.submitData.relation_id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			
			sendCode(){
				var self = this;
				console.log(111)
				if(self.hasSend){
					return;
				};
				var phone = self.submitData.phone;
				
				if (phone.trim().length != 11 || !/^1[3|4|5|6|7|8|9]\d{9}$/.test(phone)) {
					
					self.$Utils.showToast('请输入正确的手机号', 'none');
					return;
				}
				var postData = {
					data:{
						phone:self.submitData.phone
					}
					
				};
				var callback = function(res){
					if(res.solely_code==100000){
						self.hasSend = true;
						var interval = setInterval(function() {
							self.currentTime--; //每执行一次让倒计时秒数减一
						
							self.text=self.currentTime + 's';//按钮文字变成倒计时对应秒数
							
							//如果当秒数小于等于0时 停止计时器 且按钮文字变成重新发送 且按钮变成可用状态 倒计时的秒数也要恢复成默认秒数 即让获取验证码的按钮恢复到初始化状态只改变按钮文字
							if (self.currentTime <= 0) {
								clearInterval(interval)
								
								self.hasSend = false;
								self.text='重新发送';
								self.currentTime= 61;
								
							}
							
						}, 1000);
					}else{
						self.$Utils.showToast('发送失败', 'none');
					};
				};
				self.$apis.codeGet(postData, callback);
			},
			
			formIdAdd(e) {
				const self = this;
				console.log(e)
			
				self.$apis.WxFormIdAdd(e.detail.formId, Date.parse(new Date()) / 1000 + 7 * 86400);
			},
		
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data[0]
					} else {
						self.$Utils.showToast(res.msg, 'none');
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
				
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);	
				var newObject = self.$Utils.cloneForm(self.submitData);
				delete newObject.content;
				const pass = self.$Utils.checkComplete(newObject);
				if (pass) {								
					const callback = (user, res) => {
						self.messageAdd();
					};
					self.$Utils.getAuthSetting(callback);
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
					console.log(self.submitData)
				};
			},
			
			
			messageAdd() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				if(!wx.getStorageSync('user_info')||!wx.getStorageSync('user_info').headImgUrl){
					postData.refreshToken = true;
				};
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				postData.smsAuth = {
					phone:self.submitData.phone,						
					code:self.submitData.code
				};
				console.log('postData',postData)			
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
				self.$apis.messageAdd(postData, callback);
			},
			
		},
	};
</script>

<style>
	.pub_editInfor .item .rr{text-align: right;}
</style>

 
