<template>
	<view>
		<view class="index_topBj">
			<view class="banner-box">
				<view class="banner radius10 oh">
					<swiper class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000"
					 indicator-active-color="#434343">
						<block v-for="(item,index) in swiperData.mainImg" :key="index">
							<swiper-item class="swiper-item">
								<image :src="item.url" class="slide-image" />
							</swiper-item>
						</block>
					</swiper>
				</view>
			</view>
		</view>
		<view class="f5H10"></view>
		<view class="infor-title flexRowBetween" style="padding: 30rpx 4%;background: #fff;">
			<view class="tt" style="font-size: 28rpx;">积分兑换优惠券</view>
			<view class="more" @click="Router.navigateTo({route:{path:'/pages/coupon_rule/coupon_rule'}})">兑换规则&gt;</view>
		</view>

		<view class="orderNav" style="background: #f5f5f5;">
			<view class="tt" :class="current==1?'on':''" @click="change('1')">积分兑换</view>
			<view class="tt" :class="current==2?'on':''" @click="change('2')">免费领取</view>
		</view>
		<view class="cuponList" v-if="current==1">
			<view class="item" v-for="(item,index) in mainData" :key="index">
				<view class="infor">
					<view class="flex">
						<view class="left flexCenter">
							<view class="mny">{{item.type==1?item.value:item.discount/100}}</view>
						</view>
						<view class="text">
							<!-- <view>牛肉生鲜</view> -->
							<view>{{item.price?item.price:''}}积分兑换</view>
							<view>满{{item.condition?item.condition:''}}元使用</view>
						</view>
					</view>
					<view class="data">{{item.startTime?item.startTime:''}}-{{item.endTime?item.startTime:''}}</view>
				</view>
				<view class="rrTex" @click="couponAdd(index)">立即兑换</view>
			</view>
		</view>
		<view class="cuponList" v-if="current==2">
			<view class="item" v-for="(item,index) in mainData" :key="index">
				<view class="infor">
					<view class="flex">
						<view class="left flexCenter">
							<view class="mny">{{item.type==1?item.value:item.discount/100}}</view>
						</view>
						<view class="text">
							<view>{{item.title?item.title:''}}</view>
							<view style="margin-top: 30rpx;">满{{item.condition?item.condition:''}}元使用</view>
						</view>
					</view>
					<view class="data">{{item.startTime?item.startTime:''}}-{{item.endTime?item.endTime:''}}</view>
				</view>
				<view class="rrTex" @click="couponAdd(index)">立即领取</view>
			</view>
		</view>

		<!-- 兑换弹框 -->
		<view class="xieyiAlert" v-if="is_show">
			<view class="infor center" style="padding: 120rpx 30px;height: auto;border-radius: 10rpx;">
				<view class="colseBtn" @click="deltAlert" style="top: 20rpx;right: 20rpx;left:auto;">×</view>
				<view class="tit font16" style="padding-bottom: 60rpx;">确认是否删除这个技能</view>
				<view class="btnB flexRowBetween">
					<view class="on">是</view>
					<view @click="deltAlert">否</view>
				</view>
			</view>
		</view>

		<!-- 免费领取 -->

		<view class="blackAlert radius10 center" v-if="is_blackAlert">
			<view class="colseBtn" @click="blackAlert" style="top: 20rpx;right: 20rpx;left:auto;">×</view>
			<view class="tit">领取成功，可以在我的优惠券查看</view>
			<view class="okBtn" @click="blackAlert">确定</view>
		</view>

	</view>
</template>
<script>
	export default {
		data() {
			return {
				Router: this.$Router,
				swiperData: {},
				mainData: [],
				cuponList: [{}, {}],
				freeList: [{}, {}],
				current: 1,
				getBefore:{
					caseData: {
						tableName: 'Label',
						searchItem: {
							title: ['=', ['积分兑换']],
						},
						middleKey: 'category_id',
						key: 'id',
						condition: 'in',
					},
				},
				is_show: false,
				is_blackAlert: false,
			}
		},

		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getSliderData','getMainData','getUserInfoData'], self);
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
			
			couponAdd(index) {
				const self = this;
				var now =  (new Date()).getTime();
				const postData = {
					tokenFuncName: 'getProjectToken',
					coupon_id: self.mainData[index].id,
					data:{
						invalid_time:now + self.mainData[index].valid_time 
					}
				};
				if(self.current==1){
					postData.pay = {
						score:self.mainData[index].price
					}
				}else{
					postData.pay = {
						score: 0
					};
					//postData.data.pay_status=1
				};
				if(self.current==1){
					postData.saveFunction = [{
						FuncName: 'wxTemplate',
						searchItem:{
							user_no:uni.getStorageSync('user_info').user_no
						},
						template:'使用积分提醒',
						data_arr:{
							keyword1:{
								value:self.mainData[index].price+'分'
							},
							keyword2:{
								value:(parseFloat(self.userInfoData.score)-parseFloat(self.totalScore))+'分'
							},
						},
						thirdapp_id:2
					}];
				}
				console.log('postData', postData)
				const callback = (res) => {
					if (res && res.solely_code == 100000) {
						self.$Utils.showToast('领取成功', 'none')
					} else {
						self.$Utils.showToast(res.msg, 'none')
					}
				};
				self.$apis.couponAdd(postData, callback);
			},
			
			userCouponAdd(index) {
				const self = this;
				var now =  (new Date()).getTime();
				const postData = {
					tokenFuncName: 'getProjectToken',
				
					data:{
						invalid_time:now + self.mainData[index].valid_time,
						condition:self.mainData[index].condition,
						coupon_no:self.mainData[index].coupon_no,
						discount:self.mainData[index].discount,
						price:self.mainData[index].price,
						type:self.mainData[index].type,
						use_limit:self.mainData[index].use_limit,
						value:self.mainData[index].value,
						pay_status:1,
						status:1,
						use_step:1,
						snap_coupon:self.mainData[index]
					}
				};
			
				console.log('postData', postData)
				const callback = (res) => {
					if (res && res.solely_code == 100000) {
						self.$Utils.showToast('领取成功', 'none')
					} else {
						self.$Utils.showToast(res.msg, 'none')
					}
				};
				self.$apis.userCouponAdd(postData, callback);
			},

			change(current) {
				const self = this;
				if (current != self.current) {
					self.current = current
					if(self.current==1){
						self.getBefore = {
							caseData: {
								tableName: 'Label',
								searchItem: {
									title: ['=', ['积分兑换']],
								},
								middleKey: 'category_id',
								key: 'id',
								condition: 'in',
							},
						}
					}else{
						self.getBefore = {
							caseData: {
								tableName: 'Label',
								searchItem: {
									title: ['=', ['免费领取']],
								},
								middleKey: 'category_id',
								key: 'id',
								condition: 'in',
							},
						}
					};
					self.getMainData(true)
				}
			},
			chageSelt(selt) {
				const self = this;
				if (selt != self.selt) {
					self.selt = selt
				}
			},
			deltAlert() {
				const self = this;
				self.is_show = !self.is_show;
			},
			blackAlert() {
				const self = this;
				self.is_blackAlert=!self.is_blackAlert;
			},
			

			getMainData(isNew) {
				const self = this;
				var nowTime = Date.parse(new Date());
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
				postData.searchItem = {
					thirdapp_id: 2,
					start_time:['<',nowTime],
					end_time:['>',nowTime]
				};
				postData.getBefore = self.$Utils.cloneForm(self.getBefore);
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						for (var i = 0; i < self.mainData.length; i++) {
							self.mainData[i].startTime = self.$Utils.timeto(self.mainData[i].start_time,'ymd')
							self.mainData[i].endTime = self.$Utils.timeto(self.mainData[i].end_time,'ymd')
						}
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.couponGet(postData, callback);
			},

			getSliderData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					title: '优惠券轮播'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.swiperData = res.info.data[0]
					}
					console.log('self.swiperData', self.swiperData)
					self.$Utils.finishFunc('getSliderData');
				};
				self.$apis.labelGet(postData, callback);
			},
		},

	};
</script>

<style>
	@import "../../assets/style/page.css";

	page {
		background: #f5f5f5;
		padding-bottom: 60rpx;
	}

	.blackAlert {
		position: fixed;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		width: 80%;
		background: rgba(0, 0, 0, 0.5);
		color: #fff;
		padding: 90rpx 3%;
		box-sizing: border-box;
		z-index: 30;
	}

	.blackAlert .colseBtn {
		width: 40rpx;
		height: 40rpx;
		line-height: 32rpx;
		background: none;
		border: 2rpx solid #fff;
		color: #fff;
		font-size: 36rpx;
		line-height: 36rpx;
	}

	.blackAlert .tit {
		font-size: 28rpx;
		margin-bottom: 80rpx;
	}

	.blackAlert .okBtn {
		width: 140RPX;
		height: 60rpx;
		line-height: 60rpx;
		color: #222;
		border-radius: 8rpx;
		text-align: center;
		background: #F3D012;
		margin: 0 auto;
	}
</style>
