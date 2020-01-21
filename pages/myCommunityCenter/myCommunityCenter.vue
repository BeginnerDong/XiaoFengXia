<template>
	<view>
		<view class="orderNav orderNavThree">
			<view class="tt" :class="current==1?'on':''" @click="change('1')">待审核</view>
			<view class="tt" :class="current==2?'on':''" @click="change('2')">已发布</view>
			<view class="tt" :class="current==3?'on':''" @click="change('3')">关注</view>
			<view class="tt" :class="current==4?'on':''" @click="change('4')">收藏</view>
			<view class="tt" :class="current==5?'on':''" @click="change('5')">评论</view>
		</view>

		<view class="myComunityCt" v-if="mainData.length>0">
			<view class="interct_idexLis" v-if="current==2||current==1">
				<view class="child" v-for="(item,index) in mainData" :key="index" 
				@click="Router.navigateTo({route:{path:'/pages/fengxiangba_pl/fengxiangba_pl'}})">
					<view class="title font14">{{item.title}}</view>
					<view class="text font13">{{item.content}}</view>
					<view class="imgbox">
						<view v-for="c_item in item.bannerImg" :class="item.bannerImg.length==1?'lisOne':(item.bannerImg.length==2?'lisTwo':'lisThree')">
							<image :src="c_item" mode=""></image>
						</view>
					</view>
					<view class="pltex_label">
						<view class="lis">
							<image src="../../static/images/beestoenjoy-icon4.png" mode=""></image>
							<view>{{item.reply.count}}</view>
						</view>
						<view class="lis">
							<image src="../../static/images/beestoenjoy-icon3.png" mode=""></image>
							<view>{{item.like.count}}</view>
						</view>
						<view class="lis">
							<image src="../../static/images/beestoenjoy-icon2.png" mode=""></image>
							<view>{{item.collect.count}}</view>
						</view>
					</view>
				</view>
			</view>

			<view class="myRowBetween" v-if="current==3">
				<view class="item flexRowBetween radius10" v-for="(item,index) in mainData" :key="index">
					<view class="left">
						<view class="photo">
							<image :src="item.user&&item.user[0]?item.user[0].headImgUrl:''" mode=""></image>
						</view>
						<view>{{item.user&&item.user[0]?item.user[0].nickname:''}}</view>
					</view>
					<view class="right" @click="updateFollowLog(index)">
						<view class="gzOkBtn">已关注</view>
					</view>
				</view>
			</view>
			<view class="interct_idexLis" v-if="current==4||current==5">
				<view class="child" v-for="(item,index) in mainData" :key="index" 
				@click="Router.navigateTo({route:{path:'/pages/fengxiangba_pl/fengxiangba_pl'}})">
					<view class="title font14">{{item.message&&item.message[0]?item.message[0].title:''}}</view>
					<view class="text font13">{{item.message&&item.message[0]?item.message[0].content:''}}</view>
					<view class="imgbox">
						<view v-for="c_item in item.message[0].bannerImg" 
						:class="item.bannerImg.length==1?'lisOne':(item.bannerImg.length==2?'lisTwo':'lisThree')">
							<image :src="c_item" mode=""></image>
						</view>
					</view>
					<view class="pltex_label">
						<view class="lis">
							<image src="../../static/images/beestoenjoy-icon4.png" mode=""></image>
							<view>{{item.reply.count}}</view>
						</view>
						<view class="lis">
							<image src="../../static/images/beestoenjoy-icon3.png" mode=""></image>
							<view>{{item.like.count}}</view>
						</view>
						<view class="lis">
							<image src="../../static/images/beestoenjoy-icon6.png" mode=""></image>
							<view>{{item.collect.count}}</view>
						</view>
					</view>
				</view>
			</view>
			<view v-if="mainData.length==0" style="text-align: center;margin-top: 50px;">
				<image src="../../static/images/noContent.png" style="width: 300rpx;height: 300rpx;"></image>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router: this.$Router,
				current: 1,
				shimgData: [{}, {}],
				followDate: [{}, {}, {}, {}, {}],
				mainData: [],
				searchItem:{
					type:1,
					behavior:0
				}
			}
		},


		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
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
			
			change(current) {
				const self = this;
				if(current!=self.current){
					self.current = current;
					if(self.current==1){
						self.searchItem = {
							type:1,
							behavior:0,
							user_no:uni.getStorageSync('user_info').user_no
						}
						self.getMainData(true)
					}else if(self.current==2){
						self.searchItem = {
							type:1,
							behavior:1,
							user_no:uni.getStorageSync('user_info').user_no
						}
						self.getMainData(true)
					}else if(self.current==3){
						self.getFollowData(true)
					}else if(self.current==4){
						self.getCollectData(true)
					}else if(self.current==5){
						self.getReplyData(true)
					}
				}
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
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.searchItem.user_no = uni.getStorageSync('user_info').user_no;
				postData.getAfter = {
					follow:{
						tableName: 'Log',
						searchItem: {
							status:['in',[1,-1]],
							type:3,
							relation_table:'user',
							user_no:uni.getStorageSync('user_info').user_no
						},
						middleKey: 'user_no',
						key: 'relation_user',
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
						self.mainData.push.apply(self.mainData, res.info.data);
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.messageGet(postData, callback);
			},
			
			getFollowData(isNew) {
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
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					type:3,
					user_no:uni.getStorageSync('user_info').user_no
				};
				postData.getAfter = {
					user: {
						tableName: 'User',
						searchItem: {
							status:1,
						},
						middleKey: 'relation_user',
						key: 'user_no',
						condition: 'in',
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.logGet(postData, callback);
			},
			
			updateFollowLog(index) {
				const self = this;
				var now = Date.parse(new Date());
				now = self.$Utils.timeto(now,"ymd-hms");
				const postData = {
					searchItem: {
						id: self.mainData[index].id	
					},
					data: {
						status: -self.mainData[index].status
					}
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					self.canClick = true;
					if (res.solely_code == 100000) {
						self.$Utils.showToast('取消成功', 'none', 1000)
						setTimeout(function() {
							self.getFollowData(true)
						}, 1000);
					} else {
						self.$Utils.showToast(res.msg, 'none', 1000)
					};
				};
				self.$apis.logUpdate(postData, callback);
			},
			
			getCollectData(isNew) {
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
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					type:2,
					user_no:uni.getStorageSync('user_info').user_no
				};
				postData.getAfter = {
					message: {
						tableName: 'Message',
						searchItem: {
							status:1,
						},
						middleKey: 'relation_id',
						key: 'id',
						condition: 'in',
					},
						like: {
							tableName: 'Log',
							searchItem: {
								status:1,
								type:1,
								relation_table:'message'
							},
							middleKey: ['message',0,'id'],
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
							middleKey:  ['message',0,'id'],
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
							middleKey: ['message',0,'id'],
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
						self.mainData.push.apply(self.mainData, res.info.data);
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.logGet(postData, callback);
			},
			
			getReplyData(isNew) {
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
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					type:2,
					user_no:uni.getStorageSync('user_info').user_no
				};
				postData.getAfter = {
					message: {
						tableName: 'Message',
						searchItem: {
							status:1,
						},
						middleKey: 'relation_id',
						key: 'id',
						condition: 'in',
					},
					like: {
						tableName: 'Log',
						searchItem: {
							status:1,
							type:1,
							relation_table:'message'
						},
						middleKey: ['message',0,'id'],
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
						middleKey:  ['message',0,'id'],
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
						middleKey: ['message',0,'id'],
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
						self.mainData.push.apply(self.mainData, res.info.data);
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.messageGet(postData, callback);
			},
		}
	}
</script>
<style>
	@import "../../assets/style/page.css";

	page {
		background: #f5f5f5;
		padding-bottom: 80rpx;
	}

	.orderNavThree .tt {
		width: 20%;
	}

	.gzOkBtn {
		font-size: 24rpx;
		width: 120rpx;
		text-align: center;
		line-height: 44rpx;
		background: #e1e1e1;
		border-radius: 30rpx;
	}
	.interct_idexLis .imgbox .lisTwo{
		width: 48%;
		height:304rpx
	}
</style>
