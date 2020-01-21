<template>
	<view class="">
		<view class="myRowBetween" >
			<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index">
				<view class="left">
					<view>{{item.products&&item.products[0]&&item.products[0].snap_product?item.products[0].snap_product.title:''}}</view>
					<!-- <view class="time">2019-09-19</view> -->
				</view>
				<view class="right pubColor">{{item.create_time}}</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
				showView: false,
				Router:this.$Router,
				score:'',
				wx_info:{},
				rewardData:[
					{},{},{},{},{}
				],
				mainData:[],
				paginate : {
					count: 0,
					currentPage: 1,
					pagesize: 15,
					is_page: true,
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			//self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self)
		
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
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						pagesize:15,
						is_page: true,
					};
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					type:4
				};
				postData.searchItem.pay_status = 1;
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					console.log(self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},

		},
	};
</script>


 
