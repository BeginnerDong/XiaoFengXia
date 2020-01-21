<template>
	<view>
		<view class="pdlr4" v-if="mainData.length>0">
			<view class="ind_proList">
				<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index"  
				@click="Router.navigateTo({route:{path:'/pages/goodsJoinDetail/goodsJoinDetail'}})">
					<view class="ll">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
					</view>
					<view class="rr">
						<view class="avoidOverflow2 title">{{item.title}}</view>
						<view class="lable">
							<view class="tt">{{item.score}}积分</view>
							<view class="tt">{{item.standard}}人团</view>
						</view>
						<view class="money flexRowBetween">
							<view class="flex left">
								<view class="price">{{item.price}}</view>
								<view class="yuan price">{{item.o_price}}</view>
							</view>
							<view class="addbtn">开团</view>
						</view>
						
					</view>
				</view>
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
				Router: this.$Router,
				mainData:[],
				
				indProDate: [{}, {}, {}, {}, {}]
			}
		},

		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.category_id = options.id;
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
						pagesize: 5
					}
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id: 2,
					type: 1,
					category_id:self.category_id,
					level: ['>',0]
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
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

	page {
		padding-bottom: 60rpx;
	}

	.ind-cont4 {
		padding-top: 50rpx;
	}

	.ind-cont4 .list {
		white-space: nowrap;
	}

	.ind-cont4 .item {
		width: 25%;
		text-align: center;
		font-size: 26rpx;
		padding-bottom: 50rpx;
		box-sizing: border-box;
		display: inline-block;
	}

	.ind-cont4 .item image {
		width: 60rpx;
		height: 60rpx;
		display: block;
		margin: 0 auto 20rpx auto;
	}
</style>
