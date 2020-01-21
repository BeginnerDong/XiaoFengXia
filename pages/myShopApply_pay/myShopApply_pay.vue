<template>
	<view class="">
		
		<view class="shopPay">
			<view class="item boxShaow" @click="choose(index)" v-for="(item,index) in choosePay" :key="index">
				<view class="icon">
					<image :src="item.iconUrl" mode=""></image>
				</view>
				<view class="money">{{item.level}}<text class="price">{{item.price}}</text></view>
				<view class="choose">
					<image :src="curr==index?'../../static/images/pay-icon5.png':'../../static/images/pay-icon6.png'" mode=""></image>
				</view>
			</view>
			
		</view>
		<view class="submitbtn" style="margin-top: 160rpx;">
			<button class="hei" type="submit"  @click="Utils.stopMultiClick(submit)">支付</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:[],
				curr:0,
				Utils:this.$Utils,
				choosePay:[
					{
						iconUrl:'../../static/images/pay-icon1.png',
						level:'V3',
						price:''
					},
					{
						iconUrl:'../../static/images/pay-icon2.png',
						level:'V4',
						price:''
					},
					{
						iconUrl:'../../static/images/pay-icon3.png',
						level:'V5',
						price:''
					},
					{
						iconUrl:'../../static/images/pay-icon4.png',
						level:'V6',
						price:''
					}
				],
				orderList:{}
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			submit(){
				const self = this;
				uni.setStorageSync('canClick',false);
				self.addOrder()
			},
			
			addOrder() {
				const self = this;					
				const postData = self.$Utils.cloneForm(self.orderList);
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res && res.solely_code == 100000) {
						self.orderId = res.info.id;
						self.pay()
					} else {		
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};		
				};
				self.$apis.addOrder(postData, callback);
			},
				
			pay(order_id) {
				const self = this;	
				const postData = {};	
				postData.wxPay = {
					price: self.orderList.product[0].product.price
				};
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
				};
				postData.payAfter = [
					{
						tableName: 'UserInfo',
						FuncName: 'update',
						data: {
							check_status:1,
							level:self.orderList.product[0].product.level,
							vip_time:Date.parse(new Date())/1000
						},
						searchItem:{
							user_no:uni.getStorageSync('user_info').user_no
						}
					},
				];
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									uni.showToast({
										title: '支付成功',
										duration: 1000,
										success: function() {
											
										}
									});
									setTimeout(function() {
										self.$Router.switchTab({route:{path:'/pages/index/index'}})
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									uni.showToast({
										title: '支付失败',
										duration: 2000
									});
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {
							
							uni.showToast({
								title: '支付成功',
								duration: 1000,
								success: function() {
									
								}
							});
							setTimeout(function() {
								self.$Router.redirectTo({route:{path:'/pages/myOderList/myOderList'}})
							}, 1000);
						};
					} else {
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};
				};
				self.$apis.pay(postData, callback);
			},
			
			choose(index){
				const self = this;
				if(self.curr!=index){
					self.curr = index;
					self.orderList = {
						product: [{
							id: self.mainData[index].id,
							count: 1,
							product: self.mainData[index]
						}],
						type:self.mainData[index].type,
					};
				}
				
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				
				postData.searchItem = {
					thirdapp_id: 2,
					type:5,				
				};
				postData.order = {
					level:'asc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						for (var i = 0; i < self.mainData.length; i++) {
							for (var j = 0; j < self.choosePay.length; j++) {
								if(self.mainData[i].title==self.choosePay[j].level){
									 self.choosePay[j].price = self.mainData[i].price
									  self.choosePay[j].id = self.mainData[i].id
								}
							}
						}
						self.orderList = {
							product: [{
								id: self.mainData[0].id,
								count: 1,
								product: self.mainData[0]
							}],
							type:self.mainData[0].type,
						};
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
	.shopPay{padding: 40rpx 4%; display: flex;justify-content: space-between; flex-wrap: wrap;}
	.shopPay .item{ width: 48%;padding: 40rpx 30rpx;box-sizing: border-box;text-align: center;border-radius: 10rpx; margin-bottom: 30rpx;}
	.shopPay .item .icon{ width: 110rpx; height: 110rpx; margin: 0 auto;}
	.shopPay .item .icon image{width: 100%; height: 100%;}
	.shopPay .item .money{margin: 30rpx auto;}
	.shopPay .item .choose image{width: 40rpx; height: 40rpx; margin: 0 auto; display: block;}
</style>


 
