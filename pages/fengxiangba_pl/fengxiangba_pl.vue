<template>
	<view>
		<form @submit="formIdAdd" report-submit="true">
		<view class="interct_idexLis">
			<view class="child">
				<view class="flexRowBetween">
					<view class="flex">
						<view class="photo">
							<image :src="originData.mainImg&&originData.mainImg[0]?originData.mainImg[0]:''" mode="">
								
							</image>
						</view>
						<view class="font13 name avoidOverflow">{{originData.description}}</view>
					</view>
					
					<view class="gzBtn" @click="clickFollow()">{{originData.followMe.length==0?'关注ta':(originData.followMe[0].status==1?'已关注':'关注ta')}}</view>
				</view>
				<view class="title font14">{{originData.title}}</view>
				<view class="text font13">{{originData.content}}</view>
				<view class="imgbox">
					<view v-for="(item,index) in originData.bannerImg"
					style="width: 100%;">
						<image :src="item" mode="widthFix" @click="previewImage(index)"></image>
					</view>
				</view>
				<view class="pltex_label">
					<view class="lis">
						<image src="../../static/images/beestoenjoy-icon4.png" mode=""></image>
						<view>{{originData.reply.count}}</view>
					</view>
					<view class="lis" @click="Utils.stopMultiClick(clickGood)">
						<image src="../../static/images/beestoenjoy-icon3.png" mode=""></image>
						<view>{{originData.like.count}}</view>
					</view>
					<view class="lis" @click="Utils.stopMultiClick(clickCollect)">
						<image src="../../static/images/beestoenjoy-icon2.png" mode=""></image>
						<view>{{originData.collect.count}}</view>
					</view>
					<button class="lis" open-type="share">
						<image src="../../static/images/beestoenjoy-icon1.png" mode=""></image>
						<view>分享</view>
					</button>
				</view>
			</view>
		</view>
		
		<view class="f5H10"></view>
		<view class="infor-title">
			<view class="tt">全部评价</view>
		</view>
		<view class="fxPlunCont">
			<view class="child" v-for="(item,index) in mainData" :key="index">
				<view class="photo"><image :src="item.mainImg[0]" mode=""></image></view>
				<view class="infor">
					<view class="name font13">{{item.title}}</view>
					<view class="time">{{item.create_time}}</view>
					<view class="title">{{item.content}}</view>
					<view class="pl" v-if="item.reply.length>0">
						<view class="lis" v-for="(c_item,index) in item.reply" :key="index">
							<text class="plNam">{{c_item.title}}：</text>
							<text>{{c_item.content}}</text>
						</view>
						
					</view>
				</view>
				<view class="pltex_label">
					<view class="lis" :data-id="item.id"
					@click="Router.navigateTo({route:{path:'/pages/fengxiangba_plDetail/fengxiangba_plDetail?id='+$event.currentTarget.dataset.id}})">
						<image src="../../static/images/beestoenjoy-icon4.png" mode=""></image>
						<view>{{item.replyCount.count}}</view>
					</view>
					<view class="lis">
						<image src="../../static/images/beestoenjoy-icon3.png" mode=""></image>
						<view>{{item.like.count}}</view>
					</view>
				</view>
			</view>
		</view>
	
		<view class="interDtFixd flexRowBetween">
			<input type="text" value="" placeholder="说点什么" v-model="content"/>
			<button class="sendBtn" style="font-size:14px;background-color:none"
			open-type="getUserInfo" form-type="submit"  @getuserinfo="Utils.stopMultiClick(submit)">发送</button>
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
			const callback = (res) =>{
				self.$Utils.loadAll(['getOriginData','getUserInfoData'], self);
			};
			self.$Token.getProjectToken(callback,{refreshToken:true})
			
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
			
			previewImage(index){
				const self = this;
				self.isPre = true;
				console.log(index);
				uni.previewImage({
					urls: self.mainData.bannerImg,
					current:index,
					longPressActions: {
						itemList: ['发送给朋友', '保存图片', '收藏'],
						success: function(data) {
							console.log('选中了第' + (data.tapIndex + 1) + '个按钮,第' + (data.index + 1) + '张图片');
						},
						fail: function(err) {
							console.log(err.errMsg);
						}
					}
				});
			},
			
			onShareAppMessage: function(ops) {
				console.log(ops)
				const self = this;
				var index = ops.target.dataset.index;
				if (ops.from === 'button') {
					// 来自页面内转发按钮
					return {
						title: self.originData.title,
						imageUrl: self.originData.bannerImg[0].url,
						desc: '小蜂侠社区团购',
						path: '/pages/fengxiangba_pl/fengxiangba_pl?id='+self.originData.id, //点击分享的图片进到哪一个页面
						success: function(res) {
							// 转发成功
							console.log("转发成功:" + JSON.stringify(res));
						},
						fail: function(res) {
							// 转发失败
							console.log("转发失败:" + JSON.stringify(res));
						}
					}
					console.log(ops.target)
				}else{
					
				}
			},
			
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
						middleKey: 'user_no',
						key: 'relation_user',
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
					
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.originData = res.info.data[0];
						self.getMainData(true)
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					console.log('self.originData', self.originData)
					self.$Utils.finishFunc('getOriginData');
				};
				self.$apis.messageGet(postData, callback);
			},
			
			
			clickFollow() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if(self.originData.user_no==uni.getStorageSync('user_info').user_no){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('不能关注自己', 'none', 1000);
					return
				};
				if (self.originData.follow.length == 0) {
					self.addFollowLog()
				} else {
					self.updateFollowLog()
				};
			},
			
			addFollowLog(index) {
				const self = this;
				var now = Date.parse(new Date());
				now = self.$Utils.timeto(now,"ymd-hms");
				const postData = {};
				postData.data = {
					type: 3,
					title: '关注成功',
					relation_user: self.originData.user_no,
					relation_table:'User',
					user_no: uni.getStorageSync('user_info').user_no,
				};
				postData.tokenFuncName = 'getProjectToken';
				postData.saveFunction = [{
					FuncName: 'wxTemplate',
					searchItem:{
						user_no:self.originData.user_no
					},
					template:'关注提醒',
					data_arr:{
						keyword1:{
							value:self.originData.description
						},
						keyword2:{
							value:uni.getStorageSync('user_info').nickname!=''?uni.getStorageSync('user_info').nickname:'用户'+uni.getStorageSync('user_info').user_no
						},
						keyword3:{
							value:now
						},
						keyword4:{
							value:'你发布的文章，有用户关注啦!'
						}
					},
					thirdapp_id:2
				}];
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.originData.follow.push({
							status: 1,
							id: res.info.id
						});		
						self.$Utils.showToast('关注成功', 'none', 1000)
					} else {
						self.$Utils.showToast('关注失败', 'none', 1000)
					};
					uni.setStorageSync('canClick', true);	
				};
				self.$apis.logAdd(postData, callback);
			},
			
			
			updateFollowLog() {
				const self = this;
				var now = Date.parse(new Date());
				now = self.$Utils.timeto(now,"ymd-hms");
				const postData = {
					searchItem: {
						id: self.originData.follow[0].id	
					},
					data: {
						status: -self.originData.follow[0].status
					}
				};
				postData.tokenFuncName = 'getProjectToken';
				if(self.mainData[index].follow[0].status==-1){
					postData.saveFunction = [{
						FuncName: 'wxTemplate',
						searchItem:{
							user_no:self.originData.user_no
						},
						template:'关注提醒',
						data_arr:{
							keyword1:{
								value:self.originData.description
							},
							keyword2:{
								value:uni.getStorageSync('user_info').nickname!=''?uni.getStorageSync('user_info').nickname:'用户'+uni.getStorageSync('user_info').user_no
							},
							keyword3:{
								value:now
							},
							keyword4:{
								value:'你发布的文章，有用户关注啦!'
							}
						},
						thirdapp_id:2
					}];
				}
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res.solely_code == 100000) {
						self.originData.follow[0].status = -self.originData.follow[0].status;
						if(self.originData.follow[0].status==1){	
							self.$Utils.showToast('关注成功', 'none', 1000)
						}else{
							self.$Utils.showToast('取消成功', 'none', 1000)
						}
					} else {
						self.$Utils.showToast(res.msg, 'none', 1000)
					};
				};
				self.$apis.logUpdate(postData, callback);
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
					relation_id:self.originData.id,
					relation_table:'message',
					message_id:0
				};	
				postData.getAfter = {
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
	
					reply: {
						tableName: 'Message',
						searchItem: {
							status:1,
							type:2
						},
						middleKey: 'id',
						key: 'message_id',
						condition: 'in',
					},
					replyCount: {
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
					relation_id:self.originData.id,
					title:self.description,
					mainImg:self.mainImg,
					content:self.content,
					relation_table:'message'
				};
				console.log('postData',postData)
				if(parseInt(self.userInfoData.todayScore.count)<parseInt(uni.getStorageSync('user_info').thirdApp.custom_rule.score_limit)){
					postData.saveAfter = [
						{
							tableName: 'FlowLog',
							FuncName: 'add',
							data: {
								type: 3,
								relation_id: self.originData.id,
								relation_table:'message',
								trade_info:'评论奖励积分',
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
				}
				
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
							self.getUserInfoData();	
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
			
			clickCollect(e) {
				const self = this;
				uni.setStorageSync('canClick', false);	
				if (self.originData.collectMe.length == 0) {
					self.addCollectLog()
				} else {
					self.updateCollectLog()
				};
			},
			
			addCollectLog() {
				const self = this;
				const postData = {};
				postData.data = {
					type: 2,
					title: '收藏成功',
					relation_id: self.id,
					relation_table:'Message',
					user_no: uni.getStorageSync('user_info').user_no,
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.originData.collectMe.push({
							status: 1,
							id: res.info.id
						});
						self.originData.collect.count = self.originData.collect.count+1
						self.$Utils.showToast('收藏成功', 'none', 1000)
						console.log('self.originData',self.originData)
					} else {
						self.$Utils.showToast('收藏失败', 'none', 1000)
					};
					uni.setStorageSync('canClick', true);	
				};
				self.$apis.logAdd(postData, callback);
			},
			
			
			updateCollectLog() {
				const self = this;
			
				const postData = {
					searchItem: {
						id: self.originData.collectMe[0].id
						
					},
					data: {
						status: -self.originData.collectMe[0].status
					}
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res.solely_code == 100000) {
						self.originData.collectMe[0].status = -self.originData.collectMe[0].status;
						if(self.originData.collectMe[0].status==1){
							self.originData.collect.count = self.originData.collect.count+1
							self.$Utils.showToast('收藏成功', 'none', 1000)
						}else{
							self.originData.collect.count = self.originData.collect.count-1
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
