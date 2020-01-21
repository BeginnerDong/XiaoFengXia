<template>
	<view>

		<view class="pdlr4" v-for="(item,index) in mainData">
			<view class="ind_proList">
				<view class="item flexRowBetween">
					<view class="ll">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
					</view>
					<view class="rr">
						<view class="avoidOverflow2 title">{{item.title}}</view>
						<view class="money flexRowBetween">
							<view class="flex left">
								<view class="price">{{item.o_price}}</view>
								<view class="yuan price">{{item.price}}</view>
							</view>
							<button class="addbtn" open-type="getUserInfo"  @getuserinfo="submit(index)">砍价</button>
						</view>

					</view>
				</view>
			</view>
		</view>

	</view>

</template>

<script>
	export default {
		data() {
			return {
				
				Router: this.$Router,
				orderList:{},
				mainData:[]
			}
		},

		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.user_no = uni.getStorageSync('user_info').user_no;
			const callback = (res) => {
				self.$Utils.loadAll(['getMainData'], self);
			};
			self.$Token.getProjectToken(callback, {
				refreshToken: true
			})
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
			
			submit(index){
				const self = this;
				uni.setStorageSync('canClick',false);
				self.orderList = {
					product: [{
						id: self.mainData[index].id,
						count: 1,
						product: self.mainData
					}],
					type: self.mainData[index].type,
				};
				if(self.mainData[index].order.length==0){
					
					const callback = (user, res) => {
						self.addOrder()
					};
					self.$Utils.getAuthSetting(callback);
				}else{
					self.Router.navigateTo({route:{path:'/pages/game_bargainDetailTwo/game_bargainDetailTwo?id='+self.orderList.product[0].id}})
				}
			},
			
			addOrder(){
				const self  = this;
				
				const postData = self.$Utils.cloneForm(self.orderList)
				postData.tokenFuncName = 'getProjectToken';
				
				if(!wx.getStorageSync('user_info')||!wx.getStorageSync('user_info').headImgUrl){
					postData.refreshToken = true;
				};
				const callback = (res) => {
					if (res && res.solely_code == 100000) {
						self.Router.navigateTo({route:{path:'/pages/game_bargainDetailTwo/game_bargainDetailTwo?id='+self.orderList.product[0].id}})
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
					type: 3,
				};
				postData.getAfter = {
					order: {
						token:uni.getStorageSync('user_token'),
						tableName: 'Order',
						searchItem: {
							pay_status:0,
							status:1,
							user_no:uni.getStorageSync('user_info').user_no
						},
						middleKey: 'product_no',
						key: 'product_no',
						condition: 'in'
					},
				}
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
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
