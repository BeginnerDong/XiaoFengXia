<template>
	<view class="" >
		<form @submit="formIdAdd" report-submit="true">
		<view style="background: #fff;">
			
			<view class="ind_proList">
				<view class="item flexRowBetween" v-for="(item,index) in mainData.product" :key="index">
					<view class="ll">
						<image :src="item.product&&item.product.mainImg&&item.product.mainImg[0]?item.product.mainImg[0].url:''" mode=""></image>
					</view>
					<view class="rr">
						<view class="avoidOverflow2 title">{{item.product?item.product.title:''}}</view>
						<!-- <view class="lable">
							<view class="tt">3斤装</view>
						</view> -->
						<view class="money flexRowBetween">
							<view class="flex left">
								<view class="price">{{item.product?item.product.price:''}}</view>
							</view>
							<view class="numBox" style="position: absolute; right: 0; bottom: 0;">
								<view @click="counter(index,'-')">-</view>
								<view class="num">{{item.count}}</view>
								<view @click="counter(index,'+')">+</view>
							</view>
						</view>
					</view>
				</view>
			</view>
			
			
			<view class="cfmSetAdrs flexRowBetween borderB1" style="align-items: normal;">
				<view class="yy-title">积分抵扣</view>
				<view class="yy-adrs flexRowBetween">
					<view class="cont font13">
						<view class="" >当前可用积分：{{userInfoData.score}}</view>
					</view>
					<view class="arrow" style="padding: 0;" >
						<image class="" style="width: 40rpx; height:40rpx;" src="../../static/images/pay-icon5.png" alt=""/>
					</view>
				</view>
			</view>
			
			<view class="cfmSetAdrs flexRowBetween borderB1" style="align-items: normal;">
				<view class="yy-title">收货地址</view>
				<view class="yy-adrs flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/myAddress/myAddress'}})">
					<view class="cont font13" v-if="addressData.city">
						<view class="name" >{{addressData.name}}&nbsp;&nbsp;&nbsp;&nbsp;{{addressData.phone}}</view>
						<view class="" >{{addressData.city+addressData.detail}}</view>
					</view>
					<view class="cont font13" v-else>
						请选择收货地址
					</view>
					<view class="arrow">
						<image class=" arrowR" src="../../static/images/home-icon3.png" alt=""/>
					</view>
				</view>
			</view>
			<view class="cfmSetAdrs flexRowBetween">
				<view class="fonot14">运费</view>
				<view class="font14 price">{{transportFee}}</view>
			</view>
			
		</view>
		
		<view class="confm_BFix">
			合计： <view class="price">{{totalPrice}}</view>
			<view class="btn"   @click="Utils.stopMultiClick(submit)">立即购买</view>
		</view>
		</form>
	</view>

</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils: this.$Utils,
				addressData:{},
				mainData:[],
				totalPrice:0,
				userInfoData:{},
				transportFee:0,
				specialProvince:[],
				totalScore:0
			}
		},

		onLoad(options) {
			const self = this;
			uni.setStorageSync('canClick',true);
			self.mainData = uni.getStorageSync('payPro');
			console.log('self.data.mainData', self.mainData);
			const callback = (res) =>{
				self.$Utils.loadAll(['getUserInfoData'], self);	
			};
			self.$Token.getProjectToken(callback,{refreshToken:true})
			
			self.countTotalPrice();
			self.specialProvince = uni.getStorageSync('user_info').thirdApp.codeName.split('-');
			console.log('self.specialProvince',self.specialProvince)
		},

		onShow() {
			const self = this;
			if(uni.getStorageSync('choosedAddressData')){
				self.addressData = uni.getStorageSync('choosedAddressData')
				if(self.specialProvince.indexOf(self.addressData.province)>=0){
					self.transportFee = uni.getStorageSync('user_info').thirdApp.custom_rule.special 
				}else{
					self.transportFee = uni.getStorageSync('user_info').thirdApp.custom_rule.freight 
				}
			}else{
				self.getAddressData()
			}
		},

		methods: {
			
			
			formIdAdd(e) {
				const self = this;
				console.log(e)
				self.$apis.WxFormIdAdd(e.detail.formId, Date.parse(new Date()) / 1000 + 7 * 86400);
			},
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};		
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData=res.info.data[0];	
					} else {
						self.$Utils.showToast(res.msg,'none');
					};
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},	
			
			submit(){
				const self = this;
				uni.setStorageSync('canClick',false);
				if(JSON.stringify(self.addressData) == '{}'){
					uni.setStorageSync('canClick',true);
					self.$Utils.showToast('请选择收货地址','none')
				}else{
					if(parseFloat(self.userInfoData.score)<parseFloat(self.totalScore)){
						uni.setStorageSync('canClick',true);
						self.$Utils.showToast('积分不足','none');
						return
					}
					self.addOrder()
				}
			},
			
			addOrder() {
				const self = this;					
				const postData = self.$Utils.cloneForm(self.mainData)
				postData.tokenFuncName = 'getProjectToken';
				postData.snap_address = self.addressData;
				postData.data = {}
				postData.data.transport_type  = 1;
				postData.data.transport_fee  = self.transportFee;
				postData.snap_address = self.addressData;
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
			
			/* getOrderData() {
				const self = this;
				const postData = {};		
				postData.searchItem = {
					id:self.orderId,
					user_type:0
				};		
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.orderData=res.info.data[0];
						
					} else {
						self.$Utils.showToast(res.msg,'none');
					};
				};
				self.$apis.orderGet(postData, callback);
			},	 */
			
		
			
			pay(order_id) {
				const self = this;	
				const postData = {};	
				postData.wxPay = {
					price: (parseFloat(self.totalPrice) + parseFloat(self.transportFee) - parseFloat(self.totalScore)).toFixed(2)
				};
				postData.score = {
					price:self.totalScore
				};
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
				};	
				postData.saveFunction = [{
					FuncName: 'wxTemplate',
					searchItem:{
						user_no:uni.getStorageSync('user_info').user_no
					},
					template:'使用积分提醒',
					data_arr:{
						keyword1:{
							value:self.totalScore+'分'
						},
						keyword2:{
							value:(parseFloat(self.userInfoData.score)-parseFloat(self.totalScore)).toFixed(2)+'分'
						},
					},
					thirdapp_id:2
				}];
				/* postData.payAfter = [
					{
						tableName: 'FlowLog',
						FuncName: 'add',
						data: {
							count:self.totalScore,
							user_no:uni.getStorageSync('user_info').user_no,
							thirdapp_id:2,
							
						},
					},
				]; */
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
										self.$Router.redirectTo({route:{path:'/pages/myOrder/myOrder'}})
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
								self.$Router.redirectTo({route:{path:'/pages/myOrder/myOrder'}})
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
			
			getAddressData() {
				const self = this;		
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					isdefault:1
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.addressData = res.info.data[0];
						if(self.specialProvince.indexOf(self.addressData.province)>=0){
							console.log(21321)
							self.transportFee = uni.getStorageSync('user_info').thirdApp.custom_rule.special 
						}else{
							self.transportFee = uni.getStorageSync('user_info').thirdApp.custom_rule.freight 
						}
					}
				};
				self.$apis.addressGet(postData, callback);
			},
			
			counter(index,type) {
				const self = this;			
				if (type == '+') {
					self.mainData.product[index].count++;
				} else {
					if (self.mainData.product[index].count > 1) {
						self.mainData.product[index].count--;
					}
				};			
				self.countTotalPrice();
			},
			
			countTotalPrice() {
				const self = this;
				self.totalPrice = 0;		
				self.totalScore = 0;
				for (var i = 0; i < self.mainData.product.length; i++) {
					self.totalPrice += self.mainData.product[i].product.price * self.mainData.product[i].count;
					self.totalScore += self.mainData.product[i].product.score * self.mainData.product[i].count;
				};
			},
			
			
		}
	}
</script>


<style>
	@import "../../assets/style/page.css";
	@import "../../assets/style/car.css";
	page{background: #f5f5f5;padding-bottom: 140rpx;}

</style>
