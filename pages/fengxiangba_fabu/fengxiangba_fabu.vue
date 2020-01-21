<template>
	<view>
		<view class="pub_editColumn">
			<view class="item">
				<input type="text" value="" placeholder="给你的文章写个标题吧~" v-model="submitData.title"/>
			</view>
			<view class="item">
				<textarea value="" placeholder=""  placeholder="发点你想说的~" v-model="submitData.content"/>
			</view>
			<view class="item" style="display: flex;">
				<view class="upPic" v-for="item in submitData.bannerImg" v-if="submitData.bannerImg.length>0">
					<image :src="item" mode=""></image>
					
				</view>
				<view class="upPic" @click="upLoadImg()" v-if="submitData.bannerImg.length<3">
					
					
					<image src="../../static/images/release-icon2.png" mode=""></image>
					
				</view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 200rpx;">
			<button class="hei" type="submit" open-type="getUserInfo"  @getuserinfo="Utils.stopMultiClick(submit)">确认发布</button>
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
					/* title:'',
					mainImg:'', */
					title:'',
					content:'',
					type:1,
					bannerImg:[],
					keywords:'',
					
				}
			}
		},
		
		onLoad() {
			const self = this;
			
			uni.setStorageSync('canClick', true);
			const callback = (res) =>{
				self.$Utils.loadAll(['getLocation','getUserInfoData'], self);
			};
			self.$Token.getProjectToken(callback,{refreshToken:true})
			
		},
		
		methods: {
			
			formIdAdd(e) {
				const self = this;
				console.log(e)
			
				self.$apis.WxFormIdAdd(e.detail.formId, Date.parse(new Date()) / 1000 + 7 * 86400);
			},
			
			getUserInfoData() {
				const self = this;
				var dayStart = new Date(new Date().setHours(0, 0, 0, 0)).getTime() / 1000;
				var nowTime = Date.parse(new Date()) / 1000;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				postData.getAfter = {
					todayScore: {
						tableName: 'FlowLog',
						searchItem: {
							create_time:['between',[dayStart,nowTime]],
							status:1,
							type:3,
							user_no:wx.getStorageSync('user_info').user_no,
							relation_table:'message'
						},
						middleKey: 'user_no',
						key: 'user_no',
						condition: 'in',
						compute:{						  
						  count:[
						    'sum',
						    'count',
						    {
						      create_time:['between',[dayStart,nowTime]],
						      status:1,
						      type:3,
						      user_no:wx.getStorageSync('user_info').user_no,
						      relation_table:'message',
							  count:['>',0]
						    }
						  ]
						},
					},
				}
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0];
						
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					console.log('self.getUserInfoData', self.getUserInfoData)
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			
			getLocation() {
				const self = this;
				const callback = (res) => {
					if (res) {
						console.log('res', res)
						if(res.authSetting){
							console.log(232)
							return
						}
						
						self.submitData.keywords = res.address_component.city
						
					};
				};
				self.$Utils.getLocation('reverseGeocoder', callback);
				self.$Utils.finishFunc('getLocation');
			},
			
			upLoadImg(type) {
				const self = this;			
				wx.showLoading({
					mask: true,
					title: '上传中',
				});
				const callback = (res) => {
					console.log('res', res)
					if (res.solely_code == 100000) {
						
						self.submitData.bannerImg.push(res.info.url)
						console.log(self.submitData)
						wx.hideLoading()
					} else {
						self.$Utils.showToast('网络故障', 'none')
					}
				};				
				wx.chooseImage({
					count: 1,
					success: function(res) {
						console.log(res);
						var tempFilePaths = res.tempFilePaths[0];
						console.log(callback)
						self.$Utils.uploadFile(tempFilePaths, 'file', {
							tokenFuncName: 'getProjectToken',
							type:'image'
						}, callback)
					},
					fail: function(err) {
						wx.hideLoading();
					},			
				})			
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);			
				const pass = self.$Utils.checkComplete(self.submitData);

				if (pass) {								
					const callback = (user, res) => {
						self.submitData.mainImg = [];
						self.submitData.description = user.nickName;
						self.submitData.mainImg.push(user.avatarUrl);
						self.messageAdd();
						console.log('user',user)
						console.log('res',res)
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
				var now = Date.parse(new Date());
				now = self.$Utils.timeto(now,"ymd-hms");
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				if(!wx.getStorageSync('user_info')||!wx.getStorageSync('user_info').headImgUrl){
					postData.refreshToken = true;
				};
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				postData.data.behavior = 0;
				console.log('postData',postData)
				/* if(parseInt(self.userInfoData.todayScore.count)<parseInt(uni.getStorageSync('user_info').thirdApp.custom_rule.score_limit)){
					postData.saveAfter = [
						{
							tableName: 'FlowLog',
							FuncName: 'add',
							data: {
								type: 3,
								relation_id:0,
								relation_table:'message',
								trade_info:'发布帖子奖励积分',
								user_no:uni.getStorageSync('user_info').user_no,
								thirdapp_id:2,
								count:uni.getStorageSync('user_info').thirdApp.custom_rule.publish
							}
						}
					];	
					if(parseInt(self.userInfoData.todayScore.count)+parseInt(uni.getStorageSync('user_info').thirdApp.custom_rule.publish)>parseInt(uni.getStorageSync('user_info').thirdApp.custom_rule.score_limit)){
						postData.saveAfter[0].data.count = parseInt(uni.getStorageSync('user_info').thirdApp.custom_rule.score_limit)-parseInt(self.userInfoData.todayScore.count)
					}
					postData.saveFunction = [{
						FuncName: 'wxTemplate',
						searchItem:{
							user_no:uni.getStorageSync('user_info').user_no
						},
						template:'获得积分提醒',
						data_arr:{
							keyword1:{
								value:'发布帖子奖励积分'
							},	
							keyword2:{
								value:uni.getStorageSync('user_info').thirdApp.custom_rule.publish
							},
							keyword3:{
								value:now
							},
							
						},
						thirdapp_id:2
					}];
				}; */
				
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('发布成功', 'none');
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
	@import "../../assets/style/page.css";

	page {padding-bottom: 60rpx;}
	
</style>
