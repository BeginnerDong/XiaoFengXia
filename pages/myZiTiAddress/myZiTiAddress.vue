<template>
	<view>
		
		<view class="myRowBetween" >
			<view class="item flexRowBetween" v-if="mainData.length>0" v-for="(item,index) in mainData"   :key="index"  @click="choose(index)">
				<view class="left">
					<view class="flex">
						<view>{{item.shop_name}}</view>
						<view class="font12 color3 iphone">{{item.phone}}</view>
					</view>
					<view class="time">{{item.address}}</view>
				</view>
				<view class="right">
					<image class="setIcon" :src="curr==index?'../../static/images/address-icon3.png':'../../static/images/address-icon4.png'" mode=""></image>
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
				Router:this.$Router,
				mainData:[],
				rewardData:[
					{},{},{}
				],
				curr:-1
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
			
			
			
			choose(index){
				const self = this;
				self.curr = index
				uni.setStorageSync('chooseShopData', self.mainData[index]);
				uni.navigateBack({
					delta:1
				})
			},
			
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_type:1
				};
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData.push.apply(self.mainData,res.info.data)
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getMainData');

				};
				self.$apis.userInfoGet(postData, callback);

			},

		},
	};
</script>

<style>
	@import "../../assets/style/user.css";
	.myRowBetween .item .left{width: 90%;}
	.myRowBetween .item .right{width: 5%;}
	.setIcon{padding: 20rpx 0; width: 30rpx; height: 30rpx; display: block;}
	.iphone{margin-left: 20rpx;}
	
</style>
