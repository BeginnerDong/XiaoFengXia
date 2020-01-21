<template>
	<view>
		<view class="myExtendTop">
			<view class="bigNum">{{userInfoData.balance}}</view>
			<view class="yuan">佣金</view>
			<view class="txBtn"  @click="Router.navigateTo({route:{path:'/pages/myCashOut/myCashOut?type='+type}})">提现</view>
		</view>
		
		<view class="myRowBetween" v-if="mainData.length>0">
			<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index">
				<view class="left">
					<view class="avoidOverflow">{{item.trade_info}}<span style="font-size:12px;color: red;" v-if="item.status==0">(审核中)</span></view>
					<view class="time">{{item.create_time}}</view>
				</view>
				<view class="right">{{item.count}}</view>
			</view>
		</view>
		<view v-if="mainData.length==0" style="text-align: center;margin-top: 50px;">
			<image src="../../static/images/noContent.png" style="width: 300rpx;height: 300rpx;"></image>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				rewardData:[
					{},{},{}
				],
				searchItem:{
					type:2,
					status:['in',[0,1]]
				},
				userInfoData:{},
				mainData:[],
				type:'',
				paginate:{
					count: 0,
					currentPage: 1,
					pagesize: 10,
					is_page: true,
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.type = options.type;
			//self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			//self.$Utils.loadAll(['getMainData'], self)
		
		},
			
		onShow() {
			const self =  this;
			self.getUserInfoData();
			self.getMainData(true);
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
				console.log('852369')
				const postData = {
					searchItem:{}
				};
				if(self.type=='merchant'){
					postData.tokenFuncName = 'getMerchantToken'
					postData.searchItem.user_no = uni.getStorageSync('merchantInfo').user_no
				}else{
					postData.tokenFuncName = 'getProjectToken'
					postData.searchItem.user_no = uni.getStorageSync('user_info').user_no
				};		
				
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.userInfoData = res.info.data[0];
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						pagesize: 10,
						is_page: true,
					};
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				if(self.type=='merchant'){
					postData.tokenFuncName = 'getMerchantToken'
				}else{
					postData.tokenFuncName = 'getProjectToken'
				};		
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log(self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.flowLogGet(postData, callback);
			},

		},
	};
</script>

<style>
	@import "../../assets/style/user.css";
	
</style>
