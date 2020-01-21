<template>
	<view>
		<view class="tooling_indNav">
			<view class="list">
				<view class="item" :class="currentType==2?'on':''" @click="changeone('2')">自提订单</view>
				<view class="item" :class="currentType==1?'on':''" @click="changeone('1')">快递订单</view>
			</view>
		</view>
		<view v-if="currentType==2">
			<view class="orderNav orderNavThree">
				<view class="tt" :class="currentStatus==1?'on':''" @click="change('1')">全部</view>
				<view class="tt" :class="currentStatus==2?'on':''" @click="change('2')">待核销</view>
				<view class="tt" :class="currentStatus==3?'on':''" @click="change('3')">已完成</view>
			</view>
			<view class="prolisbox pdlr4" v-if="mainData.length>0">
				<view class="prolis boxShaow" v-for="(item,index) in mainData">
					<view class="datt">
						<view class="left">
							<view class="color2" style="margin-bottom: 10rpx;">订单编号：{{item.order_no}}</view>
						</view>
						<view class="state" v-if="item.check_status==0">待核销</view>
						<view class="state" v-if="item.check_status==1">已完成</view>
					</view>
					<view class="datt">
						<view class="left">
							<view class="color2" style="margin-bottom: 10rpx;">自提门店：{{item.shop?item.shop.shop_name:''}}</view>
						</view>
					</view>
					
					<view class="ind_proList">
						<view class="item flexRowBetween" v-if="item.products&&item.products[0]&&item.products[0].product_id==0">
							<view class="ll">
								<image :src="item.products&&item.products[0]&&item.products[0].snap_product&&
								item.products[0].snap_product.product&&item.products[0].snap_product.product.mainImg
								&&item.products[0].snap_product.product.mainImg[0]?item.products[0].snap_product.product.mainImg[0].url:''"
								 mode=""></image>
							</view>
							<view class="rr">
								<view class="avoidOverflow2 title">{{item.products&&item.products[0]&&item.products[0].snap_product&&
								item.products[0].snap_product.product?item.products[0].snap_product.product.title:''}}</view>
								<view class="lable">
									<view class="tt">{{item.products&&item.products[0]?item.products[0].title:''}}</view>
								</view>
								<view class="money flexRowBetween">
									<view class="flex left">
										<view class="price">{{item.price}}</view>
									</view>
								</view>
					
							</view>
						</view>
						<view class="item flexRowBetween"  v-if="item.products&&item.products[0]&&item.products[0].sku_id==0">
							<view class="ll">
								<image :src="item.products&&item.products[0]&&item.products[0].snap_product&&
								item.products[0].snap_product&&item.products[0].snap_product.mainImg
								&&item.products[0].snap_product.mainImg[0]?item.products[0].snap_product.mainImg[0].url:''"
								 mode=""></image>
							</view>
							<view class="rr">
								<view class="avoidOverflow2 title">{{item.products&&item.products[0]?item.products[0].title:''}}</view>
								<!-- <view class="lable">
									<view class="tt">{{item.products&&item.products[0]?item.products[0].title:''}}</view>
								</view> -->
								<view class="money flexRowBetween">
									<view class="flex left">
										<view class="price">{{item.price}}</view>
									</view>
								</view>
						
							</view>
						</view>
					</view>
					<view class="bBtn">
						<view class="btn" v-if="item.order_step==5||item.order_step==0" 
						:data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/myOrder_hexiao/myOrder_hexiao?id='+$event.currentTarget.dataset.id}})">核销码</view>
						<button class="btn" open-type="share" v-if="item.order_step==4" 
						style="color: #f3d012;height: 54rpx;line-height: 54rpx;" :data-index="index" @click="onShareAppMessage()">未成团，去分享</button>
						<view class="btn" v-if="item.check_status==1&&item.order_step!=3&&item.transport_status==0" @click="orderUpdate(index,'3')">确认收货</view>
						<view class="btn" v-if="item.transport_status!=2&&item.order_step!=1&&item.order_step!=2&&item.order_step!=5" @click="orderUpdate(index,'1')">申请退款</view>
						<view class="btn" v-if="item.order_step==1">退款中</view>
						<view class="btn" v-if="item.order_step==2">已退款</view>
						<view class="btn" v-if="item.order_step==5">已成团</view>
					</view>
				</view>
			</view>
			<view v-if="mainData.length==0" style="text-align: center;margin-top: 50px;">
				<image src="../../static/images/noContent.png" style="width: 300rpx;height: 300rpx;"></image>
			</view>
		</view>
		<view v-if="currentType==1">
			<view class="orderNav orderNavThree">
				<view class="tt" :class="currentStatus==1?'on':''" @click="change('1')">全部</view>
				<view class="tt" :class="currentStatus==2?'on':''" @click="change('2')">待发货</view>
				<view class="tt" :class="currentStatus==3?'on':''" @click="change('3')">已发货</view>
				<view class="tt" :class="currentStatus==4?'on':''" @click="change('4')">已完成</view>
			</view>
			<view class="prolisbox pdlr4" v-if="mainData.length>0">
				<view class="prolis boxShaow"  v-for="(item,index) in mainData">
					<view class="datt">
						<view class="left">
							<view class="color2" style="margin-bottom: 10rpx;">订单编号：{{item.order_no}}</view>
						</view>
						<view class="state" v-if="item.transport_status==0">待发货</view>
						<view class="state" v-if="item.transport_status==1">待发货</view>
						<view class="state" v-if="item.transport_status==2">已完成</view>
					</view>
					<view class="ind_proList">
						<view class="item flexRowBetween" v-if="item.products&&item.products[0]&&item.products[0].product_id==0">
							<view class="ll">
								<image :src="item.products&&item.products[0]&&item.products[0].snap_product&&
								item.products[0].snap_product.product&&item.products[0].snap_product.product.mainImg
								&&item.products[0].snap_product.product.mainImg[0]?item.products[0].snap_product.product.mainImg[0].url:''"
								 mode=""></image>
							</view>
							<view class="rr">
								<view class="avoidOverflow2 title">{{item.products&&item.products[0]&&item.products[0].snap_product&&
								item.products[0].snap_product.product?item.products[0].snap_product.product.title:''}}</view>
								<view class="lable">
									<view class="tt">{{item.products&&item.products[0]?item.products[0].title:''}}</view>
								</view>
								<view class="money flexRowBetween">
									<view class="flex left">
										<view class="price">{{item.price}}</view>
									</view>
								</view>

							</view>
						</view>
						<view class="item flexRowBetween"  v-if="item.products&&item.products[0]&&item.products[0].sku_id==0">
							<view class="ll">
								<image :src="item.products&&item.products[0]&&item.products[0].snap_product&&
								item.products[0].snap_product&&item.products[0].snap_product.mainImg
								&&item.products[0].snap_product.mainImg[0]?item.products[0].snap_product.mainImg[0].url:''"
								 mode=""></image>
							</view>
							<view class="rr">
								<view class="avoidOverflow2 title">{{item.products&&item.products[0]?item.products[0].title:''}}</view>
								<!-- <view class="lable">
									<view class="tt">{{item.products&&item.products[0]?item.products[0].title:''}}</view>
								</view> -->
								<view class="money flexRowBetween">
									<view class="flex left">
										<view class="price">{{item.price}}</view>
									</view>
								</view>
						
							</view>
						</view>
					</view>
					<view class="bBtn">		
						<view class="btn" v-if="item.transport_status==1" @click="orderUpdate(index,'3')">确认收货</view>
						<button class="btn" open-type="share" v-if="item.order_step==4"
						style="color: #f3d012;height: 54rpx;line-height: 54rpx;" :data-index="index" @click="onShareAppMessage()">未成团，去分享</button>
						<view class="btn" v-if="item.order_step==1">退款中</view>
						<view class="btn" v-if="item.transport_status!=2&&item.order_step!=1&&item.order_step!=2&&item.order_step!=5" @click="orderUpdate(index,'1')">申请退款</view>
						<view class="btn" v-if="item.order_step==2">已退款</view>
						<view class="btn" v-if="item.order_step==5">已成团</view>
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
				mainData: [],
				searchItem: {
					transport_type: 2
				},
				currentStatus: 1,
				currentType: 2
			}
		},

		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			//self.$Utils.loadAll(['getMainData'], self)

		},

		onShow() {
			const self = this;
			self.mainData = [];
			self.getMainData(true)
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
			
			onShareAppMessage(ops) {
				console.log(ops)
				const self = this;
				var index = ops.target.dataset.index;
				if (ops.from === 'button') {
					// 来自页面内转发按钮
					return {
						title: self.mainData[index].products[0].snap_product.product.title?self.mainData[index].products[0].snap_product.product.title:'',
						imageUrl: self.mainData[index].products[0].snap_product.product.mainImg[0]&&self.mainData[index].products[0].snap_product.product.mainImg[0].url?self.mainData[index].products[0].snap_product.product.mainImg[0].url:'',
						desc: uni.getStorageSync('user_info').nickname+'邀请您参与团购',
						path: '/pages/goodsJoinDetail/goodsJoinDetail?id='+self.mainData[index].products[0].snap_product.product.id+'&order_id='+self.mainData[index].id, //点击分享的图片进到哪一个页面
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
						title: self.mainData[index].products[0].snap_product.product.title,
						imageUrl: self.mainData[index].products[0].snap_product.product.mainImg[0].url,
						desc: '小蜂侠社区团购',
						path: '/pages/goodsJoinDetail/goodsJoinDetail?id='+self.mainData[index].products[0].snap_product.product.id, //点击分享的图片进到哪一个页面
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
			
			
			change(currentStatus) {
				const self = this;
				if (self.currentType == 2) {
					if (currentStatus != self.currentStatus) {
						self.currentStatus = currentStatus
						if (self.currentStatus == 1) {
							delete self.searchItem.check_status
						} else if (self.currentStatus == 2) {
							self.searchItem.check_status = 0
						} else if (self.currentStatus == 3) {
							self.searchItem.check_status = 1
						}
						self.getMainData(true)
					}
				} else {
					self.currentStatus = currentStatus
					if (self.currentStatus == 1) {
						delete self.searchItem.transport_status
					} else if (self.currentStatus == 2) {
						self.searchItem.transport_status = 0
					} else if (self.currentStatus == 3) {
						self.searchItem.transport_status = 1
					} else if (self.currentStatus == 4) {
						self.searchItem.transport_status = 2
					}
					self.getMainData(true)
				}

			},

			changeone(currentType) {
				const self = this;
				if (currentType != self.currentType) {
					delete self.searchItem.transport_status;
					self.currentStatus = 1;
					self.currentType = currentType;
					self.searchItem.transport_type = self.currentType;
					if (self.searchItem.check_status) {
						delete self.searchItem.check_status
					};
					self.getMainData(true);

				};

			},

			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						pagesize: 5,
						is_page: true,
					};
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.searchItem.user_no = uni.getStorageSync('user_info').user_no;
				postData.searchItem.pay_status = 1;
				postData.tokenFuncName = 'getProjectToken';
				postData.getAfter = {
					shop:{
						tableName:'UserInfo',
						middleKey:'shop_no',
						key:'user_no',
						searchItem:{
							status:1
						},
						condition:'=',
						info:['shop_name']
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					} 
					console.log(self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
			orderUpdate(index,type) {
				const self = this;
			
				const postData = {};
				postData.data = {};
				postData.searchItem  = {
					id:self.mainData[index].id
				};
				if(type==3){
					postData.data.transport_status = 2
				};
				if(self.mainData[index].order_step!=5){
					postData.data.order_step = type
				};
				postData.tokenFuncName = 'getProjectToken';
				console.log('postData',postData)
				
				const callback = (res) => {
					if(res.solely_code==100000){
						self.$Utils.showToast('操作成功', 'none');
						setTimeout(function() {
							self.getMainData(true)
						}, 1000);
						
					}else{
						self.$Utils.showToast(res.msg, 'none');
					}
				};
				self.$apis.orderUpdate(postData, callback);
			},
		}
	}
</script>

<style>
	@import "../../assets/style/page.css";
	@import "../../assets/style/user.css";

	page {
		background: #f5f5f5;
		padding-bottom: 80rpx;
	}
	button{
		background: none;
		line-height: 1.5;
		margin: 0;
	}
	button::after{
		border: none;
	}
	.button-hover{
		color: #000000;
		background: none;
	}
	.tooling_indNav .list .item {
		width: 50%;
	}

	.prolis3 .cont {
		padding: 20rpx 0;
		box-sizing: border-box;
	}

	.prolis3 .tex {
		margin-top: 20rpx;
	}

	.orderNavThree .tt {
		width: 33.3%;
	}

	.prolisbox .prolis .ind_proList .item {
		padding: 0;
		border-bottom: 0;
	}

	.prolisbox .prolis .ind_proList .item .ll {
		width: 192rpx;
		height: 192rpx;
	}

	.prolisbox .prolis .ind_proList .item .rr {
		width: 66%;
		height: 192rpx;
	}
</style>
