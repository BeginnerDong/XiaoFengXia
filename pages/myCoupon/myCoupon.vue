<template>
	<view>
		<view class="orderNav orderNavThree">
			<view class="tt" :class="current==1?'on':''" @click="change('1')">待使用</view>
			<view class="tt" :class="current==2?'on':''" @click="change('2')">已使用</view>
			<view class="tt" :class="current==3?'on':''" @click="change('3')">已过期</view>
		</view>
		
		<view class="cuponList" v-if="current==1">
			<view class="item" v-for="(item,index) in mainData" :key="index" >
				<view class="infor">
					<view class="flex">
						<view class="left flexCenter">
							<view class="mny">{{item.type==1?item.value:item.discount/100}}</view>
						</view>
						<view class="text">
							<view>{{item.snap_coupon?item.snap_coupon.title:''}}</view>
							<view>{{item.price}}积分兑换</view>
							<view>满{{item.condition}}使用</view>
						</view>
					</view>
					<view class="data">{{item.create_time}}-{{item.invalid_time}}</view>
				</view>
				<view class="rrTex">待使用</view>
			</view>
		</view>
		<view class="cuponList" v-if="current==2">
			<view class="item" v-for="(item,index) in mainData" :key="index" >
				<view class="infor">
					<view class="flex">
						<view class="left flexCenter">
							<view class="mny">{{item.type==1?item.value:item.discount/100}}</view>
						</view>
						<view class="text">
							<view>{{item.snap_coupon?item.snap_coupon.title:''}}</view>
							<view>{{item.price}}积分兑换</view>
							<view>满{{item.condition}}使用</view>
						</view>
					</view>
					<view class="data">{{item.createTime}}-{{item.invalidTime}}</view>
				</view>
				<view class="rrTex">已使用</view>
				<view class="fixIcon"><image src="../../static/images/coupons-icon3.png" mode=""></image></view>
			</view>
		</view>
		<view class="cuponList" v-if="current==3">
			<view class="item" v-for="(item,index) in mainData" :key="index" >
				<view class="infor">
					<view class="flex">
						<view class="left flexCenter">
							<view class="mny">{{item.type==1?item.value:item.discount/100}}</view>
						</view>
						<view class="text">
							<view>{{item.snap_coupon?item.snap_coupon.title:''}}</view>
							<view>{{item.price}}积分兑换</view>
							<view>满{{item.condition}}使用</view>
						</view>
					</view>
					<view class="data">{{item.createTime}}-{{item.invalidTime}}</view>
				</view>
				<view class="rrTex">已过期</view>
				<view class="fixIcon"><image src="../../static/images/coupons-icon2.png" mode=""></image></view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				current:1,
				cuponList:[
					{},{},{}
				],
				searchItem:{
					use_step:1
				},
				mainData:[]
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
					self.current = current
					if(self.current==1){
						self.searchItem.use_step=1
					}else if(self.current==2){
						self.searchItem.use_step=2
					}else(
						self.searchItem.use_step=-1
					)
					self.getMainData(true)
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
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						for (var i = 0; i < self.mainData.length; i++) {
							self.mainData[i].createTime = self.mainData[i].create_time.substr(0,10);
							self.mainData[i].invalidTime = self.$Utils.timeto(self.mainData[i].invalid_time,'ymd')
						}
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userCouponGet(postData, callback);
			},
		}
	}
</script>

<style>
	@import "../../assets/style/page.css";
	page{ background: #f5f5f5;padding-bottom: 80rpx;}

	.orderNavThree .tt{ width: 33.3%;}
</style>
