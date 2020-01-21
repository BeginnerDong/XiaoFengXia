<template>
	<view>
		
		<view class="detailxqBan">
			<image :src="mainData.bannerImg&&mainData.bannerImg[0]?mainData.bannerImg[0].url:''" mode=""></image>
		</view>
		
		<view class="detail_one pr">
			<view class="">
				<view class="price font14">{{mainData.o_price}}</view>
			</view>
			<button open-type='share' class="flexRowAround shareBtn" style="justify-content: flex-end;">
				<image src="../../static/images/beestoenjoy-icon1.png" mode=""></image>
				<view class="" style="">分享</view>
			</button>
			<view class="flexRowBetween color2 font13 number">
				<view class="">库存：{{mainData.stock?mainData.stock:''}}</view>
				<view class="">销量：{{mainData.sale_count?mainData.sale_count:''}}</view>
			</view>
			<view class="tit font14">{{mainData.title}}</view>
		</view>
		<view class="f5H10"></view>
		<view class="detail_join" v-if="logData.length>0">
			<view class="flexRowBetween">
				<view class="font14">好友砍价</view>
				<!-- <view class="font13 color3" @click="Router.navigateTo({route:{path:'/pages/game_bargainDetailTwo_friends/game_bargainDetailTwo_friends'}})">查看更多&gt;</view> -->
			</view>
			<view class="twolist">
				<view class="item flexRowBetween" v-for="item in logData">
					<view class="ll flex">
						<view class="photo">
							<image :src="item.user&&item.user[0]?item.user[0].headImgUrl:''" mode=""></image>
						</view>
						<view class="tit avoidOverflow">{{item.user&&item.user[0]?item.user[0].nickname:''}}</view>
					</view>
					<view class="rr red">-{{item.price}}元</view>
				</view>
				
			</view>
		</view>
		<view class="detail_join" v-else>
			<view>还没有好友帮你砍价，快去分享吧</view>
		</view>
		<view class="f5H10"></view>
		<view class="infor-title">
			<view class="tt">详情介绍</view>
		</view>
		<view class="detailTexCont" style="padding: 20rpx 0;">
			<view class="center">
				<view class="content ql-editor" style="padding: 0;line-height: 0;" v-html="mainData.content">
				</view>
			</view>
		</view>
		
		<!-- 底部菜单按钮 -->
		<view class="xqbotomBar" v-if="!isShare"> 
			<view class="left flexRowBetween">
				<view class="ite" style="width:100%" @click="prev">
					<image src="../../static/images/details=icon1.png" mode=""></image>
					<view>返回</view>
				</view>
			</view>
			<view class="payBtn flexRowBetween" @click="goBuy('1')">
				<view class="item hei" style="width: 100%;">
					<view class="price">{{mainData.order&&mainData.order[0]?mainData.order[0].price:''}}</view>
				</view>
			</view>
		</view>
		
		<!-- 分享出去帮好友砍价按钮  -->
		<view class="xqbotomBar"  v-if="isShare&&mainData.logMe.length==0">
			<view class="left flexRowBetween">
				<view class="ite" style="width:100%;" @click="prev">
					<image src="../../static/images/details=icon1.png" mode=""></image>
					<view>返回</view>
				</view>
			</view>
			<view class="payBtn flexRowBetween">
				<view class="item hei" style="width: 100%;" @click="kanjia">
					<view class="pubColor">帮好友砍价</view>
				</view>
			</view>
		</view>
		
		<!-- 分享出去已帮好友砍过价 -- >
		<view class="xqbotomBar" v-if="isShare&&mainData.logMe.length!=0">
			<view class="left flexRowBetween">
				<view class="ite" style="width:100%;" @click="prev">
					<image src="../../static/images/details=icon1.png" mode=""></image>
					<view>返回</view>
				</view>
			</view>
			<view class="payBtn flexRowBetween">
				<view class="item" style="width: 100%; background: #ddd;">
					<view class="color3">已砍价</view>
				</view>
			</view>
		</view>
		
		<!-- 砍价弹框 -->
		<view class="PinDanAlert" v-if="is_show">
			<view class="explain center" style="padding: 60rpx;">
				<view class="colseBtn" @click="goKanJia" style="left: auto;right: 10rpx;top: 20rpx;">×</view>
				<view class="tit">帮好友成功砍掉<text class="red">{{num}}元</text></view>
				
				<view class="submitbtn">
					<button type="button" style="width: 100%; margin:20rpx 0;" @click="goKanJia">确定</button>
				</view>
			</view>
		</view>
		
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				is_show:false,
				show_addok:false,
				user_no:'',
				isShare:false,
				mainData:{},
				num:'',
				logData:[]
			}
		},
		
		
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			if (options.user_no) {
				self.user_no = options.user_no;
				if(self.user_no!=uni.getStorageSync('user_info').user_no){
					self.isShare =true
				}
			}else{
				self.user_no = uni.getStorageSync('user_info').user_no;
			}

			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			goBuy(type) {
				const self = this;
				var orderList = {
					sku: [{
						id: self.mainData.id,
						sku: self.mainData
					}],
					type: self.mainData.type,
				};
				uni.setStorageSync('payPro', orderList);
				self.$Router.navigateTo({
					route: {
						path: '/pages/confirmOrder/confirmOrder?type=' + type + '&order_id='
						+ self.mainData.order[0].id+'&price='+self.mainData.order[0].price
					}
				})
			},
			
			onShareAppMessage: function(ops) {
				console.log(ops)
				const self = this;
				if (ops.from === 'button') {
					// 来自页面内转发按钮
					return {
						title: '帮我砍一刀~~',
						imageUrl: self.mainData.mainImg[0].url,
						desc: '小蜂侠社区团购',
						path: '/pages/game_bargainDetailTwo/game_bargainDetailTwo?id='+self.mainData.id+'&user_no='+self.user_no, //点击分享的图片进到哪一个页面
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
					return {
						title: self.mainData.title,
						imageUrl: self.mainData.mainImg[0].url,
						desc: '小蜂侠社区团购',
						path: '/pages/game_bargainDetailTwo/game_bargainDetailTwo?id='+self.mainData.id, //点击分享的图片进到哪一个页面
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
				}
				
			},
			
			goKanJia(){
				const self = this;
				self.is_show = !self.is_show
			},
			
			prev(){
				const self = this;
				self.$Router.switchTab({
					route: {
						path: '/pages/index/index'
					}
				})
			},	
			
			kanjia(){
				const self = this;
				var max = parseFloat(self.mainData.o_price);
				var min = parseFloat(self.mainData.price);
				var num = parseFloat(Math.random()*(max-min+1)+min,10)/2;
				self.num = parseFloat(num).toFixed(2)
				self.newPrice = (parseFloat(self.mainData.order[0].price) - parseFloat(num)).toFixed(2)
				if(parseInt(self.mainData.people)<=self.logData.length){
					self.$Utils.showToast('已达到限制人数', 'none');
				}else{
					if(parseInt(self.mainData.people)-self.logData.length==1){
						self.num =(parseFloat(self.mainData.order[0].price) - parseFloat(max)).toFixed(2)
						self.newPrice = max
					};
					self.orderUpdate()
				}
			},
			
			orderUpdate() {
				const self = this;
				const postData = {};
				postData.data = {
					price:self.newPrice
				};
				postData.searchItem  = {
					id:self.mainData.order[0].id,
					user_type:0
				};
				postData.tokenFuncName = 'getProjectToken';
				console.log('postData',postData)
				postData.saveAfter = [
					{
						tableName: 'Log',
						FuncName: 'add',
						data: {
							type: 5,
							relation_id:self.mainData.order[0].id,
							relation_table:'order',
							trade_info:'砍价记录',
							user_no:uni.getStorageSync('user_info').user_no,
							thirdapp_id:2,
							price:self.num
						}
					}
				];
				const callback = (res) => {
					if(res.solely_code==100000){
						self.is_show = true;
						self.getMainData()
					}else{
						self.$Utils.showToast(res.msg, 'none');
					}
				};
				self.$apis.orderUpdate(postData, callback);
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type: 3,
					id: self.id
				};
				postData.getAfter = {
					
					order: {
						token: uni.getStorageSync('user_token'),
						tableName: 'Order',
						middleKey: 'product_no',
						key: 'product_no',
						searchItem: {
							status: 1,
							type:3,
							user_no:self.user_no
						},
						condition: '='
					},
					log: {
						tableName: 'Log',
						middleKey: ['order',0,'id'],
						key: 'relation_id',
						searchItem: {
							status: 1,
							type: 5,
							relation_table:'order'
						},
						condition: '='
					},
					logMe: {
						tableName: 'Log',
						middleKey: ['order',0,'id'],
						key: 'relation_id',
						searchItem: {
							status: 1,
							type: 5,
							relation_table:'order',
							user_no:uni.getStorageSync('user_info').user_no
						},
						condition: '='
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					self.getLogData()
					console.log('self.mainData', self.mainData)
				};
				self.$apis.productGet(postData, callback);
			},
			
			getLogData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type:5,
					relation_id:self.mainData.order[0].id,
					user_type:0
				};
				postData.tokenFuncName = 'getProjectToken';
				postData.getAfter = {	
					user: {
						tableName: 'User',
						middleKey: 'user_no',
						key: 'user_no',
						searchItem: {
							status: 1,
						},
						condition: '='
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.logData.push.apply(self.logData,res.info.data)
					} 
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.logGet(postData, callback);
			},
			
			
		},
	};
</script>

<style>
	@import "../../assets/style/page.css";
	page{padding-bottom: 140rpx;}
	
	
	.payBtn .hei .price::before{content: "总计：";}
	.shareBtn{font-size: 24rpx;padding: 0 20rpx;border-radius:30rpx 0 0 30rpx; line-height:46rpx;background: #eee; color: #666; margin-left: 6rpx;position: absolute; right: 0;top: 20rpx;}
	.shareBtn image{width: 26rpx; height: 29rpx;display: block;margin-right: 6rpx;}
</style>
