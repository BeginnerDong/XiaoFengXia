<template>
	<view>
		<form @submit="formIdAdd" report-submit="true">
		<view class="fxPlunCont">
			<view class="child" style="border-bottom: 0;">
				<view class="photo"><image :src="originData.mainImg&&originData.mainImg[0]?originData.mainImg[0]:''" mode=""></image></view>
				<view class="infor">
					<view class="name font13">{{originData.description}}</view>
					<view class="time">{{originData.create_time}}</view>
					<view class="title">{{originData.content}}</view>
					<view class="pl" v-if="mainData.length>0">
						<view class="lis" v-for="(item,index) in mainData" :key="index">
							<text class="plNam">{{item.title}}：</text>
							<text>{{item.content}}</text>
						</view>
					</view>
				</view>
				<view class="pltex_label">
					<view class="lis">
						<image src="../../static/images/beestoenjoy-icon4.png" mode=""></image>
						<view>{{originData.reply.count}}</view>
					</view>
					<view class="lis"  @click="Utils.stopMultiClick(clickGood)">
						<image src="../../static/images/beestoenjoy-icon3.png" mode=""></image>
						<view>{{originData.like.count}}</view>
					</view>
				</view>
			</view>
		</view>
	
		<view class="interDtFixd flexRowBetween">
			<input type="text" value="" placeholder="说点什么" v-model="content"/>
			<button class="sendBtn" form-type="submit" open-type="getUserInfo" style="font-size:14px;background-color:none"  @getuserinfo="Utils.stopMultiClick(submit)">发送</button>
		</view>
		</form>
	</view>
</template>

<script>
	
	export default {
		data() {
			
			return {
				Router:this.$Router,
				mainData:[],
				originData:{},
				Utils:this.$Utils,
				content:''
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getOriginData','getUserInfoData'], self);
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()	
			};
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
			
			getOriginData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id:self.id
				};
				postData.getAfter = {
					followMe: {
						tableName: 'Log',
						searchItem: {
							status:['in',[1,-1]],
							type:3,
							user_no:wx.getStorageSync('user_info').user_no
						},
						middleKey: 'id',
						key: 'relation_id',
						condition: 'in',
					},
					likeMe: {
						tableName: 'Log',
						searchItem: {
							status:['in',[1,-1]],
							type:1,
							user_no:wx.getStorageSync('user_info').user_no
						},
						middleKey: 'id',
						key: 'relation_id',
						condition: 'in',
					},
					collectMe: {
						tableName: 'Log',
						searchItem: {
							status:['in',[1,-1]],
							type:2,
							user_no:wx.getStorageSync('user_info').user_no
						},
						middleKey: 'id',
						key: 'relation_id',
						condition: 'in',
					},
					like: {
						tableName: 'Log',
						searchItem: {
							status:1,
							type:1,
							relation_table:'message'
						},
						middleKey: 'id',
						key: 'relation_id',
						condition: 'in',
						compute:{
						  
						  count:[
						    'count',
						    'count',
						    {
						      status:1,
						    }
						  ]
						},
					},
					collect: {
						tableName: 'Log',
						searchItem: {
							status:1,
							type:2,
							relation_table:'message'
						},
						middleKey: 'id',
						key: 'relation_id',
						condition: 'in',
						compute:{
						  
						  count:[
						    'count',
						    'count',
						    {
						      status:1,
						    }
						  ]
						},
					},
					reply: {
						tableName: 'Message',
						searchItem: {
							status:1,
							type:2
						},
						middleKey: 'id',
						key: 'message_id',
						condition: 'in',
						compute:{
						  
						  count:[
						    'count',
						    'count',
						    {
						      status:1,
						    }
						  ]
						},
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.originData = res.info.data[0];
						self.getMainData(true)
					}
					console.log('self.originData', self.originData)
					self.$Utils.finishFunc('getOriginData');
				};
				self.$apis.messageGet(postData, callback);
			},
			
			
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 5
					}
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					thirdapp_id: 2,
					user_type:0,
					type:2,
					message_id:self.originData.id,
					relation_table:'message'
				};	
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.messageGet(postData, callback);
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);			
				if(self.content!=''){
					var pass = true
				};	
				if (pass) {								
					const callback = (user, res) => {
						self.mainImg = [];
						self.description = user.nickName;
						self.mainImg.push(user.avatarUrl);
						self.messageAdd();
						console.log('user',user)
						console.log('res',res)
					};
					self.$Utils.getAuthSetting(callback);
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('评论不能为空', 'none')
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
				postData.data = {
					type:2,
					relation_id:self.originData.relation_id,
					title:self.description,
					mainImg:self.mainImg,
					content:self.content,
					relation_table:'message',
					message_id:self.originData.id
				};
				console.log('postData',postData)
				if(parseInt(self.userInfoData.todayScore.count)<parseInt(uni.getStorageSync('user_info').thirdApp.custom_rule.score_limit)){
					postData.saveAfter = [
						{
							tableName: 'FlowLog',
							FuncName: 'add',
							data: {
								type: 3,
								relation_id: self.originData.relation_id,
								relation_table:'message',
								trade_info:'评论奖励积分',
								user_no:uni.getStorageSync('user_info').user_no,
								thirdapp_id:2,
								count:uni.getStorageSync('user_info').thirdApp.custom_rule.favor,
								
							}
						}
					];	
					if(parseInt(self.userInfoData.todayScore.count)+parseInt(uni.getStorageSync('user_info').thirdApp.custom_rule.favor)>
					parseInt(uni.getStorageSync('user_info').thirdApp.custom_rule.score_limit)){
						postData.saveAfter[0].data.count = uni.getStorageSync('user_info').thirdApp.custom_rule.score_limit-self.userInfoData.todayScore.count
					};
					postData.saveFunction = [{
						FuncName: 'wxTemplate',
						searchItem:{
							user_no:uni.getStorageSync('user_info').user_no
						},
						template:'获得积分提醒',
						data_arr:{
							keyword1:{
								value:'评论奖励积分'
							},
							keyword2:{
								value:uni.getStorageSync('user_info').thirdApp.custom_rule.favor
							},
							keyword3:{
								value:now
							},
							
						},
						thirdapp_id:2
					}];
				};
				
				console.log('232',self.userInfoData.todayScore.count+uni.getStorageSync('user_info').thirdApp.custom_rule.favor)
				console.log('211',uni.getStorageSync('user_info').thirdApp.custom_rule.score_limit)
		
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('评论成功', 'none');
						self.content = '';
						setTimeout(function() {
							self.getOriginData();
							self.getUserInfoData();
						}, 1000);
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.messageAdd(postData, callback);
			},
			
			clickGood(e) {
				const self = this;
				uni.setStorageSync('canClick', false);	
				if (self.originData.likeMe.length == 0) {
					self.addGoodLog()
				} else {
					self.updateGoodLog()
				};
			},
			
			addGoodLog() {
				const self = this;
				var now = Date.parse(new Date());
				now = self.$Utils.timeto(now,"ymd-hms");
				const postData = {};
				postData.data = {
					type: 1,
					title: '点赞成功',
					relation_id: self.id,
					relation_table:'Message',
					user_no: uni.getStorageSync('user_info').user_no,
				};
				postData.tokenFuncName = 'getProjectToken';
				if(parseInt(self.userInfoData.todayScore.count)<parseInt(uni.getStorageSync('user_info').thirdApp.custom_rule.score_limit)){
					postData.saveAfter = [
						{
							tableName: 'FlowLog',
							FuncName: 'add',
							data: {
								type: 3,
								relation_id: self.originData.id,
								relation_table:'message',
								trade_info:'点赞奖励积分',
								user_no:uni.getStorageSync('user_info').user_no,
								thirdapp_id:2,
								count:uni.getStorageSync('user_info').thirdApp.custom_rule.favor
							}
						}
					];
					if(parseInt(self.userInfoData.todayScore.count)+parseInt(uni.getStorageSync('user_info').thirdApp.custom_rule.favor)>
					parseInt(uni.getStorageSync('user_info').thirdApp.custom_rule.score_limit)){
						postData.saveAfter[0].data.count = uni.getStorageSync('user_info').thirdApp.custom_rule.score_limit-self.userInfoData.todayScore.count
					}
					postData.saveFunction = [{
						FuncName: 'wxTemplate',
						searchItem:{
							user_no:uni.getStorageSync('user_info').user_no
						},
						template:'获得积分提醒',
						data_arr:{
							keyword1:{
								value:'点赞奖励积分'
							},
							keyword2:{
								value:uni.getStorageSync('user_info').thirdApp.custom_rule.favor
							},
							keyword3:{
								value:now
							},
							
						},
						thirdapp_id:2
					}];
				}
				if(parseInt(self.userInfoData.todayScore.count)+parseInt(uni.getStorageSync('user_info').thirdApp.custom_rule.favor)>
				parseInt(uni.getStorageSync('user_info').thirdApp.custom_rule.score_limit)){
					postData.saveAfter[0].data.count = uni.getStorageSync('user_info').thirdApp.custom_rule.score_limit-uni.getStorageSync('user_info').thirdApp.custom_rule.favor
				}
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.originData.likeMe.push({
							status: 1,
							id: res.info.id
						});
						self.originData.like.count = self.originData.like.count+1
						self.$Utils.showToast('点赞成功', 'none', 1000)
						console.log('self.originData',self.originData)
						setTimeout(function() {
							self.getUserInfoData()
						}, 1000);		
					} else {
						self.$Utils.showToast('点赞失败', 'none', 1000)
					};
					uni.setStorageSync('canClick', true);	
				};
				self.$apis.logAdd(postData, callback);
			},
			
			
			updateGoodLog() {
				const self = this;
			
				const postData = {
					searchItem: {
						id: self.originData.likeMe[0].id
						
					},
					data: {
						status: -self.originData.likeMe[0].status
					}
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res.solely_code == 100000) {
						self.originData.likeMe[0].status = -self.originData.likeMe[0].status;
						if(self.originData.likeMe[0].status==1){
							self.originData.like.count = self.originData.like.count+1
							self.$Utils.showToast('点赞成功', 'none', 1000)
						}else{
							self.originData.like.count = self.originData.like.count-1
							self.$Utils.showToast('取消成功', 'none', 1000)
						}
					} else {
						self.$Utils.showToast(res.msg, 'none', 1000)
					};
				};
				self.$apis.logUpdate(postData, callback);
			},

		},
	};
</script>


<style>
	@import "../../assets/style/page.css";
	button{
		background: none;
		line-height: 1.5;
	}
	button::after{
		border: none;
	}
	.button-hover{
		color: #000000;
		background: none;
	}
	page {padding-bottom: 140rpx;}
	
	
	
	
</style>
