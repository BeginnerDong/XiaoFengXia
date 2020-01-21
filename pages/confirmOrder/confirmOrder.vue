<template>
	<view class="" >
		<form @submit="formIdAdd" report-submit="true">
		<view style="background: #fff;">
			<view class="ind_proList">
				<view class="item flexRowBetween" v-if="!isBargain" v-for="(item,index) in mainData.sku" :key="index">
					<view class="ll">
						<image :src="item.sku&&item.sku.product&&item.sku.product.mainImg&&item.sku.product.mainImg[0]?item.sku.product.mainImg[0].url:''" mode=""></image>
					</view>
					<view class="rr">
						<view class="avoidOverflow2 title">{{item.sku&&item.sku.product?item.sku.product.title:''}}</view>
						<view class="lable">
							<view class="tt">{{item.sku&&item.sku.product?item.sku.product.score:''}}积分</view>
							<view class="tt">{{item.sku&&item.sku.product?item.sku.product.standard:''}}人团</view>
						</view>
						<view class="money flexRowBetween">
							
							<view class="flex left">
								<view class="price" v-if="type==1">{{item.sku?item.sku.price:''}}</view>
								<view class="price" v-if="type==2">{{item.sku?item.sku.group_price:''}}</view>
							</view>
							<view class="numBox" style="position: absolute; right: 0; bottom: 0;">
								<view @click="counter(index,'-')">-</view>
								<view class="num">{{item.count}}</view>
								<view @click="counter(index,'+')">+</view>
							</view>
						</view>
					</view>
				</view>
				<view class="item flexRowBetween" v-if="isBargain"  v-for="(item,index) in mainData.sku" :key="index">
					<view class="ll">
						<image :src="item.sku&&item.sku.mainImg&&item.sku.mainImg[0]?item.sku.mainImg[0].url:''" mode=""></image>
					</view>
					<view class="rr">
						<view class="avoidOverflow2 title">{{item.sku&&item.sku?item.sku.title:''}}</view>
						<view class="money flexRowBetween">
							<view class="flex left">
								<view class="price" v-if="type==1">{{item.sku?item.sku.price:''}}</view>
								<view class="price" v-if="type==2">{{item.sku?item.sku.group_price:''}}</view>
							</view>
						</view>
					</view>
				</view>
			</view>
			
			<view class="pdlr4 borderB1 flexRowBetween cfmSetAdrs">
				<view class="fonot14">优惠券</view>
				<view v-if="couponData.length==0"  class="font14 color3" style="width: 80%;text-align: right;">
					暂无优惠券使用
				</view>
				<view v-if="couponData.length>0&&chooseCoupon.length==0" @click="isShowCoupon" class="font14 color3" style="width: 80%;text-align: right;">
					请选择
				</view>
				<view  v-if="couponData.length>0&&chooseCoupon.length>0" @click="isShowCoupon" class="font14 color3" style="color:red;width: 80%;text-align: right;">
					优惠券抵扣-{{pay.coupon[0].price}}
				</view>
				<image src="../../static/images/home-icon1.png" style="width: 20rpx;height: 20rpx;"></image>
			</view>
			<view class="cfmSetAdrs flexRowBetween" v-if="showCoupon" v-for="(item,index) in couponData" @click="useCoupon(index)">
				<image class="icon"  alt="" style="width: 20rpx;height: 20rpx;"
				:src="index==couponIndex?'../../static/images/pay-icon5.png':'../../static/images/pay-icon6.png'"
				></image>
				{{item.type==1?'抵扣券':'折扣券'}}{{item.type==2?item.discount/10+'折':item.value+'元'}}
			</view>
			<view class="tihuoSelt flex">
				<view :class="num==1?'on':''" @click="changeType('1')">提货点自提</view>
				<view :class="num==2?'on':''" @click="changeType('2')">快递配送</view>
			</view>
			<view class="" v-if="num==1">
				<view class="cfmSetAdrs flexRowBetween" style="align-items: normal;">
					<view class="yy-title">提货点</view>
					<view class="yy-adrs flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/myZiTiAddress/myZiTiAddress'}})">
						<view class="cont font13" v-if="shopData.shop_name">
							<view class="name" style="width: 100%;">{{shopData.shop_name}}&nbsp;&nbsp;&nbsp;&nbsp;{{shopData.phone}}</view>
							<view class="" >{{shopData.address}}</view>
						</view>
						<view class="cont font13" v-else>
							请选择自提门店
						</view>
						<view class="arrow">
							<image class=" arrowR" src="../../static/images/home-icon3.png" alt=""/>
						</view>
						
					</view>
				</view>
			</view>
			<view v-if="num==2">
				<view class="cfmSetAdrs flexRowBetween borderB1" style="align-items: normal;">
					<view class="yy-title">快递点</view>
					<view class="yy-adrs flexRowBetween"   @click="Router.navigateTo({route:{path:'/pages/myAddress/myAddress'}})">
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
			
		</view>
		
		<view class="confm_BFix">
			合计： <view class="price">{{pay.wxPay&&pay.wxPay.price?pay.wxPay.price:'0'}}</view>
			<button style="margin-right: 0;" class="btn" form-type="submit" open-type="getUserInfo"  
			@getuserinfo="Utils.stopMultiClick(submit)">{{type==1?'立即购买':'拼团购买'}}</button>
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
				type:'',
				num:1,
				shopData:{},
				pay:{
					coupon:[]
				},
				array:[],
				couponData:[],
				couponIndex:-1,
				showCoupon:false,
				group_no:'',
				leader_no:'',
				chooseCoupon:[],
				isBargain:false
			}
		},

		onLoad(options) {
			const self = this;
			uni.setStorageSync('canClick',true);
			
			var zeroNum = 1;
			var oneNum = parseInt(uni.getStorageSync('user_info').thirdApp.custom_rule.two);
			var twoNum = uni.getStorageSync('user_info').thirdApp.custom_rule.three-uni.getStorageSync('user_info').thirdApp.custom_rule.two;
			var threeNum = uni.getStorageSync('user_info').thirdApp.custom_rule.four-uni.getStorageSync('user_info').thirdApp.custom_rule.three;
			var fourNum = uni.getStorageSync('user_info').thirdApp.custom_rule.five-uni.getStorageSync('user_info').thirdApp.custom_rule.four;
			var fiveNum = uni.getStorageSync('user_info').thirdApp.custom_rule.six-uni.getStorageSync('user_info').thirdApp.custom_rule.five;
			self.mainData = uni.getStorageSync('payPro');
			console.log('self.data.mainData', self.mainData);
			self.type=options.type;
			console.log('self.type',self.type)
			self.array = [
				zeroNum,
				oneNum,
				twoNum,
				threeNum,
				fourNum,
				fiveNum,
			];
			if(options.leader_no){
				self.leader_no = options.leader_no
			};
			if(uni.getStorageSync('group_no')){
				self.group_no = uni.getStorageSync('group_no')
			};
			if(options.order_id){
				self.orderId = options.order_id;
				self.price = options.price;
				self.isBargain = true
			};
			console.log('self.array',self.array)
			
			self.specialProvince = uni.getStorageSync('user_info').thirdApp.codeName.split('-');
			console.log('self.specialProvince',self.specialProvince)
			self.$Utils.loadAll(['getUserInfoData','getUserCouponData'], self);
		},

		onShow() {
			const self = this;
			if(uni.getStorageSync('chooseShopData')){
				self.shopData = uni.getStorageSync('chooseShopData')
			};
			if(uni.getStorageSync('choosedAddressData')){
				self.addressData = uni.getStorageSync('choosedAddressData')
				if(self.specialProvince.indexOf(self.addressData.province)>=0){
					self.transportFee = uni.getStorageSync('user_info').thirdApp.custom_rule.special 
				}else{
					self.transportFee = uni.getStorageSync('user_info').thirdApp.custom_rule.freight 
				}
				
			}else{
				self.getAddressData()
			};
			if(self.specialProvince.indexOf(self.addressData.province)>=0){
				console.log(21321)
				self.transportFee = uni.getStorageSync('user_info').thirdApp.custom_rule.special 
			}else{
				self.transportFee = uni.getStorageSync('user_info').thirdApp.custom_rule.freight 
			}
			self.countTotalPrice();
			console.log('self.shopData',self.shopData)
		},

		methods: {
			
			formIdAdd(e) {
				const self = this;
				console.log(e)
				self.$apis.WxFormIdAdd(e.detail.formId, Date.parse(new Date()) / 1000 + 7 * 86400);
			},
			
			isShowCoupon(){
				const self = this;
				self.showCoupon = !self.showCoupon
			},
			
			useCoupon(index) {
				const self = this;	
				console.log(index)
				var id = self.couponData[index].id;
				self.couponIndex = -1;
				var findCoupon = self.$Utils.findItemInArray(self.couponData, 'id', id);
				var findItem = self.$Utils.findItemInArray(self.pay.coupon, 'id', id);
				console.log('findCoupon', findCoupon)
				self.showCoupon = false;
				if(self.pay.coupon.length>=1){
					self.pay.coupon = []
					self.chooseCoupon = []
				};
				if (findCoupon) {
					findCoupon = findCoupon[1];
					var findSameCoupon =  self.$Utils.findItemsInArray(self.pay.coupon, 'product_id', id);
				} else {
					self.$Utils.showToast('优惠券错误', 'none');
					return;
				};
				if (findItem) {
					self.pay.coupon.splice(findItem[0], 1);
					self.chooseCoupon = []
				} else {
					console.log('self.data.price - self.data.couponTotalPrice',self.totalPrice - self.couponTotalPrice);
					console.log('findCoupon.condition',findCoupon.condition);
					if ((self.totalPrice - self.couponTotalPrice) < findCoupon.condition) {
						self.$Utils.showToast('未达满减标准', 'none');				
						return;
					};			
					console.log('findSameCoupon.length', findSameCoupon.length)
					if (self.pay.coupon.length >= 1) {
						self.$Utils.showToast('叠加使用超限', 'none');
					
						return;
					};
					if (findCoupon.type == 1) {
						var couponPrice = findCoupon.value;
						console.log('findCoupon.discount', findCoupon.discount)
					} else if (findCoupon.type == 2) {
						
						var couponPrice = parseFloat(self.totalPrice).toFixed(2) - parseFloat(findCoupon.discount / 100 * self.totalPrice)
							.toFixed(2);
					};
					if (parseFloat(couponPrice) + parseFloat(self.couponTotalPrice) > parseFloat(self.totalPrice)) {
						couponPrice = parseFloat(self.totalPrice).toFixed(2) - parseFloat(self.couponTotalPrice).toFixed(2);
					};
					self.pay.coupon.push({
						id: id,
						price: couponPrice.toFixed(2),
					});
					self.chooseCoupon.push({
						id: id,
						price: couponPrice,
					});
					self.showCoupon = false
					self.couponIndex = index;
				};
				self.countTotalPrice();
			},
			
			getUserCouponData() {
				const self = this;
				var now = Date.parse(new Date());
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					use_step: 1,
					type: ['in', [1, 2]]
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.couponData.push.apply(self.couponData, res.info.data)
					}
					if(!self.orderId){
						self.getProductCouponData()
					}else{
						self.$Utils.finishFunc('getUserCouponData');
					}
				};
				self.$apis.userCouponGet(postData, callback);
			},
			
			getProductCouponData() {
				const self = this;
				var now = Date.parse(new Date());
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					use_step: 1,
					type: ['in', [1, 2]]
				};
				postData.getBefore={
					coupon: {
						tableName: 'CouponRelation',
						searchItem:{
							product_no :['in',[self.mainData.sku[0].sku.product.product_no]]
						},
						middleKey: 'coupon_no',
						key: 'coupon_no ',
						condition: 'in',
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.couponData.push.apply(self.couponData, res.info.data)
					}
					self.$Utils.finishFunc('getUserCouponData');
				};
				self.$apis.userCouponGet(postData, callback);
			},
			
			getUserInfoData() {
				const self = this;
				console.log('852369')
				
				var nowTime = Date.parse(new Date())/1000;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken'
				postData.searchItem = {
					user_no: uni.getStorageSync('user_info').user_no
				};
				postData.getAfter = {
					orderCount: {
						tableName: 'Order',
						searchItem: {
							create_time:['between',[uni.getStorageSync('user_info').info.vip_time,nowTime]],
							status:1,
							type:1,
							pay_status:1
						},
						middleKey: 'user_no',
						key: 'user_no',
						condition: 'in',
						compute:{						  
						  count:[
						    'count',
						    'count',
						    {
						      create_time:['between',[uni.getStorageSync('user_info').info.vip_time,nowTime]],
						      status:1,
						      type:1,
						      pay_status:1
						    }
						  ]
						},
					},
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
			
			changeType(num){
				const self = this;
				if(self.num!=num){
					self.num=num;
					self.countTotalPrice()
				}
			},
			
			submit(){
				const self = this;
				uni.setStorageSync('canClick',false);
				if(self.num==1){
					if(JSON.stringify(self.shopData) == '{}'){
						uni.setStorageSync('canClick',true);
						self.$Utils.showToast('请选择自提门店','none')
					}else{
						if(self.orderId){
							const callback = (user, res) => {
								self.updateOrder()
							};
							self.$Utils.getAuthSetting(callback);
						}else{
							const callback = (user, res) => {
								self.addOrder()
							};
							self.$Utils.getAuthSetting(callback);
						}	
					}
				}else{
					if(JSON.stringify(self.addressData) == '{}'){
						uni.setStorageSync('canClick',true);
						self.$Utils.showToast('请选择收货地址','none')
					}else{
						if(self.orderId){
							const callback = (user, res) => {
								self.updateOrder()
							};
							self.$Utils.getAuthSetting(callback);
						}else{
							const callback = (user, res) => {
								self.addOrder()
							};
							self.$Utils.getAuthSetting(callback);
						}
					}
				}
				
			},
			
			updateOrder() {
				const self = this;		
				var now = Date.parse(new Date());
				const postData = {}
				postData.tokenFuncName = 'getProjectToken';
				if(!wx.getStorageSync('user_info')||!wx.getStorageSync('user_info').headImgUrl){
					postData.refreshToken = true;
				};
				postData.data = {};
				postData.searchItem = {
					id:self.orderId
				}
				if(self.num==1){
					postData.data.transport_type  = 2;
					postData.data.shop_no = self.shopData.user_no
				}else{
					postData.data.transport_type  = 1;
					postData.data.transport_fee  = self.transportFee;
					postData.snap_address = self.addressData;
				};
				const callback = (res) => {
					if (res && res.solely_code == 100000) {
						self.payNow()
					} else {		
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};		
				};
				self.$apis.orderUpdate(postData, callback);
			},
			
			
			
			addOrder() {
				const self = this;		
				var now = Date.parse(new Date());
				const postData = self.$Utils.cloneForm(self.mainData)
				postData.tokenFuncName = 'getProjectToken';
				
				if(!wx.getStorageSync('user_info')||!wx.getStorageSync('user_info').headImgUrl){
					postData.refreshToken = true;
				};
				postData.data = {};
				if(self.num==1){
					postData.data.transport_type  = 2;
					postData.data.shop_no = self.shopData.user_no
				}else{
					postData.data.transport_type  = 1;
					postData.data.transport_fee  = self.transportFee;
					postData.snap_address = self.addressData;
				};
				if(self.type==2){
					postData.isGroup = true
					postData.data.standard = self.mainData.sku[0].sku.product.standard
					postData.data.invalid_time = self.mainData.sku[0].sku.product.duration + now
				};
				if(self.leader_no!=''){
					postData.data.leader_no = self.leader_no
				};
				if(self.group_no!=''){
					postData.group_no = self.group_no
				};
				const callback = (res) => {
					if (res && res.solely_code == 100000) {
						self.orderId = res.info.id;
						self.payNow()
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

			payNow(order_id) {
				const self = this;	
				const postData = self.$Utils.cloneForm(self.pay);	
				/* postData.wxPay = {
					price: self.totalPrice
				};
				postData.score = {
					price:self.totalScore
				}; */
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
				};
				for (var i = 0; i < self.array.length; i++) {
					if(i==parseInt(self.userInfoData.level)&&parseInt(self.array[i]) - parseInt(self.userInfoData.orderCount.count)<=1){
						postData.payAfter = [
							{
								tableName: 'UserInfo',
								FuncName: 'update',
								data: {
									level:self.userInfoData.level+1,
									vip_time:Date.parse(new Date())/1000
								},
								searchItem:{
									user_no:uni.getStorageSync('user_info').user_no
								}
							},
						];
					}
				};
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						uni.removeStorageSync('group_no');
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
					self.mainData.sku[index].count++;
				} else {
					if (self.mainData.sku[index].count > 1) {
						self.mainData.sku[index].count--;
					}
				};			
				self.countTotalPrice();
			},
			
			countTotalPrice() {
				const self = this;
				self.totalPrice = 0;	
				self.couponTotalPrice = self.$Utils.addItemInArray(self.pay.coupon, 'price');
				if(self.orderId){
					self.totalPrice = self.price
				}else{
					for (var i = 0; i < self.mainData.sku.length; i++) {
						if(self.type==1){
							self.totalPrice += self.mainData.sku[i].sku.price * self.mainData.sku[i].count;
						}else if(self.type==2){
							self.totalPrice += self.mainData.sku[i].sku.group_price * self.mainData.sku[i].count;
						}
						
						
						console.log('self.pay',self.pay)
					};
				}
				var wxPay = parseFloat(self.totalPrice) - parseFloat(self.couponTotalPrice);
				if (wxPay > 0) {
					self.pay.wxPay = {
						price: wxPay.toFixed(2),
					};
				} else {
					  delete self.pay.wxPay;	 
				};
				if(self.num==2){
					self.pay.wxPay.price = parseFloat(self.pay.wxPay.price) + parseFloat(self.transportFee);
				}
			},
			
			
		}
	}
</script>
<style>
	@import "../../assets/style/page.css";
	@import "../../assets/style/car.css";
	page{background: #f5f5f5;padding-bottom: 140rpx;}

</style>
