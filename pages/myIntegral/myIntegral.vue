<template>
	<view>
		<view class="myExtendTop">
			<view class="bigNum">{{mainData&&mainData[0]&&mainData[0].compute?mainData[0].compute.total:'0'}}</view>
			<view class="yuan">总积分</view>
		</view>
		
		<view class="jifenTwoNum flexRowBetween boxShaow radius10">
			<view class="item">
				<view class="num">{{mainData&&mainData[0]&&mainData[0].compute?mainData[0].compute.now:'0'}}</view>
				<view>可兑换积分</view>
			</view>
			<view class="item">
				<view class="num">{{mainData&&mainData[0]&&mainData[0].compute?mainData[0].compute.hasUse:'0'}}</view>
				<view>已兑换积分</view>
			</view>
		</view>
		
		<view class="myRowBetween" >
			<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index">
				<view class="left">
					<view class="avoidOverflow">{{item.trade_info}}</view>
					<view class="time">{{item.create_time}}</view>
				</view>
				<view class="right">{{item.count}}</view>
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
				paginate:{
					count: 0,
					currentPage: 1,
					pagesize: 10,
					is_page: true,
				}
			}
		},
		
		
		onLoad() {
			const self = this;
			//self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
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
			

			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					type:3,
					user_no:uni.getStorageSync('user_info').user_no
				};
				postData.getAfter = {
					compute: {
						tableName: 'FlowLog',
						middleKey: 'user_no',
						key: 'user_no',
						condition: '=',
						searchItem: {
							status: 1,
						},
						compute: {
							total: [
								'sum',
								'count',
								{
									status: 1,
									count: ['>', 0],
									type:3
								}
							],
							hasUse: [
								'sum',
								'count',
								{
									status: 1,
									count: ['<', 0],
									type:3
								}
							],
							now: [
								'sum',
								'count',
								{
									status: 1,
									type:3
								}
							],
						}
					},
				}
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.flowLogGet(postData, callback);
			},
		}
	}
</script>

<style>
	@import "../../assets/style/user.css";
	.myRowBetween .item .left{width: 70%;}
	.myRowBetween .item .right{width: 30%;}
	
</style>
