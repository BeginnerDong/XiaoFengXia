<template>
	<view>
		<view class="detailxqBan">
			<image :src="mainData.bannerImg&&mainData.bannerImg[0]?mainData.bannerImg[0].url:''" mode=""></image>
		</view>
		<view class="detail_one">
			<view class="flex">
				<view class="price font14">{{mainData.price}}</view>
				<view class="yuanJia">{{mainData.o_price}}</view>
			</view>
			<view class="flexRowBetween color2 font13 number">
				<view class="">库存：{{mainData.stock}}</view>
				<view class="">销量：{{mainData.sale_count}}</view>
			</view>
			<view class="tit font14">{{mainData.title}}</view>
		</view>
		<view class="f5H10"></view>
		<view class="detal_guige pdlr4 flexRowBetween"  @click="showSel">
			<view class="flexRowAround ll">
				<view>规格</view>
				<!-- <view class="yyDate-lis flex" v-if="noSelectSku">
					<view v-for="item in mainData.sku">{{item.title}}</view>
				</view> -->
				<view class=" flex">
					<view>已选:{{chooseSku.title}}</view>
				</view>
			</view>
			<view class="rr">
				<image class="arrowR" src="../../static/images/home-icon3.png" mode=""></image>
			</view>
		</view>
		<view class="f5H10"></view>
		<view class="infor-title">
			<view class="tt">详情介绍</view>
		</view>
		<view class="detailTexCont">
			<view class="center">
				<view class="content ql-editor" v-html="mainData.content">
				</view>
			</view>
		</view>
		
		<!-- 底部菜单按钮 -->
		<view class="xqbotomBar">
			<view class="left flexRowBetween">
				<view class="ite" @click="prev">
					<image src="../../static/images/details=icon1.png" mode=""></image>
					<view>返回</view>
				</view>
			</view>
			<view class="payBtn flexRowBetween" @click="goBuy('1')">
				<view class="item hei" style="width: 100%;" @click="showSel">
					<view class="price">{{price}}</view>
					<view class="name">购买</view>
				</view>
			</view>
		</view>
		
		<!-- 选择规格弹框 -->
		<view class="showSel" v-if="is_show1">
			<view class="showSelCont fix">
				<view class="colseBtn" @click="showSel">×</view>
				<view class="ind_proList">
					<view class="item flexRowBetween" style="padding:0rpx 0 20rpx 0;border-bottom: 0;">
						<view class="ll">
							<image :src="mainData&&mainData.mainImg&&mainData.mainImg[0]?mainData.mainImg[0].url:''" mode=""></image>
						</view>
						<view class="rr">
							<view class="avoidOverflow2 title">{{mainData.title}}</view>
							<view class="money">
								<view class="flex left">
									<view class="price">{{chooseSku.group_price}}</view>
								</view>
							</view>
		
						</view>
					</view>
				</view>
				<view class="yy-title" style="padding: 10px 0;">规格</view>
				<view class="yyDate-lis flexRowBetween" style="flex-wrap: wrap; justify-content: flex-start;">
					<view v-for="(item,index) in mainData.sku" :key="index" :class="[chooseSku.id == item.id?'on':'']" @click="seltSpecs(index)">{{item.title}}</view>
				</view>
				<view class="flexRowBetween" style="padding: 30rpx 0; border-top: 2rpx solid #e7e7e7; border-bottom: 2rpx solid #e7e7e7; margin-top: 10rpx;">
					<view class="yy-title">购买数量</view>
					<view class="numBox" style="position: relative; right: auto; bottom: auto;">
						<view @click="counter('-')">-</view>
						<view class="num">{{count}}</view>
						<view @click="counter('+')">+</view>
					</view>
				</view>
				<view class="submitbtn" style="margin-top: 60rpx;">
					<button class="hei" style="margin-bottom: 0;" type="submit" @click="showSel">确定</button>
				</view>
			</view>
		</view>
		
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				Router: this.$Router,
				showView: false,
				is_show: false,
				show_addok: false,
				is_show1: false,
				mainData: {},
				chooseSku: {},
				noSelectSku: true,
				count: 1,
				price: '',
				groupPrice: '',
				hasAdd: false,
				isVip: false
			}
		},

		onLoad(options) {
			const self = this;
			self.id = options.id;
			if (options.user_no) {
				self.user_no = options.user_no;
				self.isVip = true
			};
			if(options.leader_no){
				self.leader_no = options.leader_no
			};
			self.$Utils.loadAll(['getMainData', 'getUserInfoData'], self);
		},

		methods: {

			getUserInfoData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken'
				postData.searchItem = {
					user_no: uni.getStorageSync('user_info').user_no
				};
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.userInfoData = res.info.data[0];
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},

			addToShop() {
				const self = this;
				if (self.hasAdd) {
					self.$Utils.showToast('商品已在店铺', 'none');
					return
				};
				if (self.userInfoData.level >= 3 && self.userInfoData.check_status == 2) {
					const postData = {};
					postData.data = {
						thirdapp_id: 2,
						type: 6,
						relation_table: 'product',
						relation_id: self.id,
						user_no: uni.getStorageSync('user_info').user_no,
					};
					postData.tokenFuncName = 'getProjectToken';
					const callback = (res) => {
						if (res.solely_code == 100000) {
							self.show_addok = true
						} else {
							self.$Utils.showToast(res.msg, 'none');
						}
					};
					self.$apis.logAdd(postData, callback);
				} else {
					self.$Utils.showToast('您暂未拥有店铺', 'none');
				}
			},

			counter(type) {
				const self = this;
				if (type == '+') {
					self.count++
				} else if (type == '-' && self.count > 1) {
					self.count--
				};
				self.countTotalPrice()
			},

			countTotalPrice() {
				const self = this;
				self.price = self.chooseSku.price * self.count;
				self.groupPrice = self.chooseSku.group_price * self.count
			},

			goBuy(type) {
				const self = this;
				if (JSON.stringify(self.chooseSku) == '{}') {
					self.$Utils.showToast('商品错误', 'none', 1000);
					return;
				};
				var orderList = {
					sku: [{
						id: self.chooseSku.id,
						count: self.count,
						sku: self.chooseSku
					}],
					type: self.mainData.type,
				};
				uni.setStorageSync('payPro', orderList);
				
				if(self.leader_no){
					self.$Router.navigateTo({
						route: {
							 path: '/pages/confirmOrder/confirmOrder?type=' + type+'leader_no='+self.leader_no
						}
					})
				}else{
					self.$Router.navigateTo({
						route: {
							path: '/pages/confirmOrder/confirmOrder?type=' + type
						}
					})
				}
				
			},
			//选择商品属性
			seltSpecs(index) {
				const self = this;
				if (self.mainData.sku[index].id != self.chooseSku.id) {
					self.chooseSku = self.mainData.sku[index];
					self.count = 1;
					self.countTotalPrice()
				}

			},

			goPinDan() {
				const self = this;
				self.is_show = !self.is_show
			},

			showSel() {
				const self = this;
				self.is_show1 = !self.is_show1
			},

			showAddok() {
				const self = this;
				self.show_addok = !self.show_addok
			},

			prev() {
				const self = this;
				self.$Router.switchTab({
					route: {
						path: '/pages/index/index'
					}
				})
			},

			onShareAppMessage: function(ops) {
				console.log(ops)
				const self = this;
				if (ops.from === 'button') {
					// 来自页面内转发按钮
					return {
						title: self.mainData.title,
						imageUrl: self.mainData.mainImg[0].url,
						desc: '小蜂侠社区团购',
						path: '/pages/goodsJoinDetail/goodsJoinDetail?id='+self.mainData.id+'&leader_no='+self.user_no, //点击分享的图片进到哪一个页面
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
						desc: '小蜂侠社区团购213',
						path: '/pages/goodsJoinDetail/goodsJoinDetail?id='+self.mainData.id, //点击分享的图片进到哪一个页面
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

			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type: 1,
					id: self.id
				};
				postData.getAfter = {
					sku: {
						tableName: 'Sku',
						middleKey: 'product_no',
						key: 'product_no',
						searchItem: {
							status: 1,
						},
						condition: '='
					},
					order: {
						token: uni.getStorageSync('user_token'),
						tableName: 'Order',
						middleKey: 'product_no',
						key: 'product_no',
						searchItem: {
							status: 1,
							order_step: 4,
							pay_status: 1
						},
						condition: '='
					},
					log: {
						tableName: 'Log',
						middleKey: 'id',
						key: 'relation_id',
						searchItem: {
							status: 1,
							type: 6
						},
						condition: '='
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						if (self.mainData.sku[0]) {
							self.chooseSku = self.mainData.sku[0];
							self.countTotalPrice()
						} else {
							self.$Utils.showToast('商品信息错误', 'none');
							setTimeout(function() {
								uni.navigateBack({
									delta: 1
								})
							}, 1500);
						};
						if (self.mainData.log.length > 0) {
							self.hasAdd = true
						};
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
		},
	};
</script>

<style>
	@import "../../assets/style/page.css";
	@import "../../assets/style/car.css";
	page{padding-bottom: 140rpx;}
	.posterBtn{position: absolute; top: 80rpx; right: 0; z-index: 2; background: #F3D012;padding: 0 16rpx; border-radius: 30rpx 0 0 30rpx; font-size: 20rpx; line-height: 44rpx;}
	
	.payBtn .item{ width: 33.3%;}
	
	.showSel{width: 100%;height: 100%;background: rgba(0, 0, 0, 0.5);position: fixed;top: 0;right: 0;bottom: 0;left: 0;z-index: 10000;}
	.showSelCont {padding:40rpx 4%;background: #fff;}
	.showSelCont.fix{position: fixed;bottom: 0;left: 0;box-sizing: border-box;width: 100%;border-radius: 20rpx 20rpx 0 0;}
	.yy-title {font-size: 28rpx;}
	.yyDate-lis {max-height: 270rpx; overflow-y: auto;}
	.yyDate-lis view {width: 130rpx;height: 56rpx;font-size: 26rpx;color: #666;text-align: center;line-height: 56rpx;background: #f5f5f5;margin: 0 20rpx 30rpx 20rpx;}
	.yyDate-lis view.on{background: #F3D012; color: #333;}
	.showSel .numBox view {background: #f5f5f5;}
	.detal_guige{padding: 20rpx 4%;}
	.detal_guige .ll{width: 90%; display: flex; justify-content: flex-start;align-items: center;}
	.detal_guige .rr{width: 10%;display: flex;justify-content: flex-end;}
	.detal_guige .yyDate-lis view{margin:0 0 0 20rpx;}
</style>
