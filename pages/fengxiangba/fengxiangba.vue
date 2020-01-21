<template>
	<view>
		<form @submit="formIdAdd" report-submit="true">
		<view class="tooling_indNav">
			<view class="list">
				<view class="item" :class="num==1?'on':''" @click="changeone('1')">首页</view>
				<view class="item" :class="num==2?'on':''" @click="changeone('2')">活动</view>
			</view>
		</view>
		
		<view class="" v-if="num==1&&mainData.length>0">
			<view class="fabubtn" 
			@click="Router.navigateTo({route:{path:'/pages/fengxiangba_fabu/fengxiangba_fabu'}})">
				<button class="icon" form-type="submit">
					<image src="../../static/images/beestoenjoy-icon7.png" mode=""></image>
				</button>
				<view class="tit">发布帖子</view>
			</view>
			<view class="interct_idexLis">
				<view class="child" v-for="(item,index) in mainData" >
					<view class="flexRowBetween">
						<view class="flex">
							<view class="photo"><image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0]:''" mode=""></image></view>
							<view class="font13 name avoidOverflow">{{item.description}}</view>
						</view>
						<button form-type="submit" style="margin: 0;" class="gzBtn" 
						@click="clickFollow(index)">
							{{item.follow.length==0?'关注ta':(item.follow[0].status==1?'已关注':'关注ta')}}
						</button>

					</view>
					<view class="title font14"  :data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/fengxiangba_pl/fengxiangba_pl?id='+$event.currentTarget.dataset.id}})">{{item.title}}</view>
					<view class="text font13"  :data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/fengxiangba_pl/fengxiangba_pl?id='+$event.currentTarget.dataset.id}})">{{item.content}}</view>
					<view class="imgbox">
						<view v-for="(c_item,c_index) in item.bannerImg" :class="item.bannerImg.length==1?'lisOne':(item.bannerImg.length==2?'lisTwo':'lisThree')">
							<image :src="c_item" mode="aspectFill" @click="previewImage(index,c_index)"></image>
						</view>
					</view>
					<view class="pltex_label">
						<view class="lis"  :data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/fengxiangba_pl/fengxiangba_pl?id='+$event.currentTarget.dataset.id}})">
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
						<button  class="lis" open-type="share" :data-index="index" style="margin: 0;background: #fff;margin-left: 40rpx;border: none;">
							<image src="../../static/images/beestoenjoy-icon1.png" mode=""></image>
							<view>分享</view>
						</button>
					</view>
				</view>	
			</view>
		</view>
		
		<view class="fx_avtive pdlr4"  v-if="num==2&&mainDataTwo.length>0">
			<view class="item" v-for="item in mainDataTwo" :data-id="item.id"
			@click="Router.navigateTo({route:{path:'/pages/activityDetail/activityDetail?id='+$event.currentTarget.dataset.id}})">
				<view class="flexRowBetween">
					<view class="flex one">
						<view class="samllIcon"><image src="../../static/images/toobtain-icon2.png" mode=""></image></view>
						<view class="font10">{{item.description}}</view>
					</view>
					<view class="rIcon"><image src="../../static/images/beestoenjoy-icon8.png" mode=""></image></view>
				</view>
				<view class="infor flexRowBetween">
					<view class="bigLogo">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" alt="" />
					</view>
					<view class="title">{{item.title}}</view>
				</view>
			</view>
		</view>
		
		<view v-if="mainData.length==0&&num==1" style="text-align: center;margin-top: 50px;">
			<image src="../../static/images/noContent.png" style="width: 300rpx;height: 300rpx;"></image>
		</view>
		<view v-if="mainDataTwo.length==0&&num==2" style="text-align: center;margin-top: 50px;">
			<image src="../../static/images/noContent.png" style="width: 300rpx;height: 300rpx;"></image>
		</view>
		</form>
	</view>
	
</template>

<script>

	export default {
		data() {
			return {
				Router:this.$Router,
				mainDataTwo:[],
				num:1,
				mainData:[],
				Utils:this.$Utils,
				canClick:true,
				isPre:false
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);	
		},
		
		
		onPullDownRefresh() {
			console.log('refresh');
			const self = this;
			if(self.num==1){
				self.getMainData(true)
			}else{
				self.getMainDataTwo(true)
			};
			setTimeout(function() {
				uni.stopPullDownRefresh();
			}, 1000);
		},
		
		onShow() {
			const self = this;
			if(uni.getStorageSync('index')){
				console.log(332423)
				self.num = 2
				self.mainDataTwo = [];
				self.paginate = {
					count: 0,
					currentPage: 1,
					is_page: true,
					pagesize: 5
				};
				self.$Utils.loadAll(['getMainDataTwo'], self);
			};
			uni.removeStorageSync('index');
			console.log('self.num',self.num)
			if(self.num==1&&!self.isPre){
				self.mainData = [];
				self.paginate = {
					count: 0,
					currentPage: 1,
					is_page: true,
					pagesize: 5
				};
				self.$Utils.loadAll(['getMainData'], self);
				self.isPre = false;
			}	
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				if(self.num==1){
					self.getMainData()
				}else{
					self.getMainDataTwo()
				}
				
			};
		},
		
		methods: {
			
			previewImage(index,c_index){
				const self = this;
				self.isPre = true;
				console.log(index);
				console.log(c_index);
				uni.previewImage({
					urls: self.mainData[index].bannerImg,
					current:c_index,
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
			
			formIdAdd(e) {
				const self = this;
			
				self.$apis.WxFormIdAdd(e.detail.formId, Date.parse(new Date()) / 1000 + 7 * 86400);
			},
			
			changeone(num){
				const self=this;
				if(num!=self.num){
					self.num = num
					if(self.num==1){
						self.getMainData(true)
					}else{
						self.getMainDataTwo(true)
					}
				}
			},
			
			clickFollow(index) {
				const self = this;
				console.log('self.canClick',self.canClick)
				if(self.canClick==false){
					console.log('stop')
					return
				};
				self.canClick = false;
				console.log(index)
				if(self.mainData[index].user_no==uni.getStorageSync('user_info').user_no){
				
					self.canClick = true;
					self.$Utils.showToast('不能关注自己', 'none', 1000);
					return
				};
				if (self.mainData[index].follow.length == 0) {
					self.addFollowLog(index)
				} else {
					self.updateFollowLog(index)
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
					relation_user: self.mainData[index].user_no,
					relation_table:'User',
					user_no: uni.getStorageSync('user_info').user_no,
				};
				postData.tokenFuncName = 'getProjectToken';
				postData.saveFunction = [{
					FuncName: 'wxTemplate',
					searchItem:{
						user_no:self.mainData[index].user_no
					},
					template:'关注提醒',
					data_arr:{
						keyword1:{
							value:self.mainData[index].description
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
						for (var i = 0; i < self.mainData.length; i++) {
							if(self.mainData[i].user_no==self.mainData[index].user_no){
								self.mainData[i].follow.push({
									status: 1,
									id: res.info.id
								});		
							}
						};
						self.$Utils.showToast('关注成功', 'none', 1000)
					} else {
						self.$Utils.showToast('关注失败', 'none', 1000)
					};
					
					self.canClick = true;
				};
				self.$apis.logAdd(postData, callback);
			},
			
			
			updateFollowLog(index) {
				const self = this;
				var now = Date.parse(new Date());
				now = self.$Utils.timeto(now,"ymd-hms");
				const postData = {
					searchItem: {
						id: self.mainData[index].follow[0].id	
					},
					data: {
						status: -self.mainData[index].follow[0].status
					}
				};
				postData.tokenFuncName = 'getProjectToken';
				if(self.mainData[index].follow[0].status==-1){
					postData.saveFunction = [{
						FuncName: 'wxTemplate',
						searchItem:{
							user_no:self.mainData[index].user_no
						},
						template:'关注申请提醒',
						data_arr:{
							keyword1:{
								value:self.mainData[index].description
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
					self.canClick = true;
					if (res.solely_code == 100000) {
						for (var i = 0; i < self.mainData.length; i++) {
							if(self.mainData[i].user_no==self.mainData[index].user_no){
								self.mainData[i].follow[0].status = -self.mainData[i].follow[0].status;
							}
						};
						if(self.mainData[index].follow[0].status==1){	
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
					type:1,
					relation_id:0,
					behavior:1
				};
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
			
			getMainDataTwo(isNew) {
				const self = this;
				if (isNew) {
					self.mainDataTwo = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 5
					}
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id:2,
				};
				postData.getBefore = {
					caseData: {
						tableName: 'Label',
						searchItem: {
							title: ['=', ['活动']],
						},
						middleKey: 'menu_id',
						key: 'id',
						condition: 'in',
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainDataTwo.push.apply(self.mainDataTwo, res.info.data);
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					console.log('self.mainDataTwo',self.mainDataTwo)
					self.$Utils.finishFunc('getMainDataTwo');
				};
				self.$apis.articleGet(postData, callback);
			},
			
			onShareAppMessage: function(ops) {
				console.log(ops)
				const self = this;
				var index = ops.target.dataset.index;
				if (ops.from === 'button') {
					// 来自页面内转发按钮
					return {
						title: self.mainData[index].title,
						imageUrl: self.mainData[index].bannerImg[0].url,
						desc: '小蜂侠社区团购',
						path: '/pages/fengxiangba_pl/fengxiangba_pl?id='+self.mainData[index].id, //点击分享的图片进到哪一个页面
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
		},
	};
</script>

<style>
	@import "../../assets/style/page.css";

	page {
		padding-bottom: 60rpx;
	}
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
	.fx_avtive .item:nth-of-type(3n+1){
		background:#2e2e97
	}
	.fx_avtive .item:nth-child(3n+2){
		background:#8632b2
	} 
	.fx_avtive .item:nth-child(3n+3){
		background:#23a156
	} 
</style>
