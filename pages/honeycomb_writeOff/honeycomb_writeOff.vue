<template>
	<view>
		<view class="tooling_indNav">
			<view class="list">
				<view class="item" :class="indexOne==1?'on':''" @click="changeone('1')">未核销</view>
				<view class="item" :class="indexOne==2?'on':''" @click="changeone('2')">已核销</view>
			</view>
		</view>
		
			<view class="prolisbox pdlr4">
				<view class="prolis boxShaow" v-for="(item,index) in mainData" :key="index">
					<view class="datt">
						<view class="left">
							<view class="color2" style="margin-bottom: 10rpx;">交易时间：{{item.create_time}}</view>
						</view>
						<view class="state" v-if="item.check_status==0">未核销</view>
						<view class="state" v-if="item.check_status==1">已核销</view>
					</view>
					<view class="ind_proList">
						<view class="item flexRowBetween">
							<view class="ll">
								<image :src="item.products&&item.products[0]&&item.products[0].snap_product&&
								item.products[0].snap_product.product&&item.products[0].snap_product.product.mainImg
								&&item.products[0].snap_product.product.mainImg[0]?item.products[0].snap_product.product.mainImg[0].url:''" mode=""></image>
							</view>
							<view class="rr">
								<view class="avoidOverflow2 title">{{item.products&&item.products[0]&&item.products[0].snap_product&&
								item.products[0].snap_product.product?item.products[0].snap_product.product.title:''}}</view>
								<view class="lable">
									<view class="tt">{{item.products&&item.products[0]?item.products[0].title:''}}</view>
								</view>
								<view class="money flexRowBetween">
									<view class="flex left">
										<view class="price">{{item.price}}</view>
									</view>
								</view>
							</view>
						</view>
					</view>
					<view class="myComunityCt" style="margin: 0;">
						<view class="myRowBetween">
							<view class="item flexRowBetween">
								<view class="left flex" style="width: 60%;">
									<view class="photo">
										<image :src="item.user&&item.user[0]?item.user[0].headImgUrl:''" mode=""></image>
									</view>
									<view class="font13">
										<view class="font13 avoidOverflow">{{item.user&&item.user[0]?item.user[0].nickname:''}}</view>
										<view class="time">{{item.user&&item.user[0]?item.user[0].info.phone:''}}</view>
									</view>
								</view>
								<view class="right red font12" style="width: 40%;">
									<view class="font14">可得佣金{{item.products&&item.products[0]&&item.products[0].snap_product&&
								item.products[0].snap_product.product?item.products[0].snap_product.product.shop_reward:''}}</view>
								</view>
							</view>
						</view>
					</view>
					<view class="bBtn">
						<view class="btn" @click="notice(index)">取货提示</view>
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
				Router:this.$Router,
				Utils:this.$Utils,
				mainData:[],
				searchItem:{
					check_status:0,
					user_type:0
				},
				indexOne:1,
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.searchItem.shop_no = uni.getStorageSync('merchantInfo').user_no;
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
			
			notice(index) {
				const self = this;
				uni.setStorageSync('canClick', false);
				console.log(index)
				const postData = {};
				postData.data = {
					order_id:self.mainData[index].id
				};
				postData.tokenFuncName = 'getMerchantToken';
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if(res.solely_code==100000){
						self.$Utils.showToast('已发送短信提醒', 'none');
					}else{
						self.$Utils.showToast(res.msg, 'none');
					}
				};
				self.$apis.notice(postData, callback);
			},
			
			changeone(index){
				const self=this;
				if(index!=self.indexOne){
					self.indexOne = index
					if(self.indexOne==1){
						self.searchItem.check_status = 0
					}else{
						self.searchItem.check_status = 1
					};
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
						pagesize: 5,
						is_page: true,
					};
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.searchItem.pay_status = 1;
				postData.tokenFuncName = 'getMerchantToken';
				postData.getAfter = {
					user:{
						tableName:'User',
						middleKey:'user_no',
						key:'user_no',
						searchItem:{
							status:1
						},
						condition:'='
					}
				};
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
			
		}
	}
</script>

<style>
	@import "../../assets/style/page.css";
	@import "../../assets/style/user.css";
	page{ background: #f5f5f5;padding-bottom: 50rpx;}

	.tooling_indNav .list .item{width: 50%;}
	.prolisbox .prolis{ margin: 0 auto 30rpx auto;}
	.myComunityCt .myRowBetween .item{padding: 0;margin: 30rpx 0 0 0;}
	
	.prolisbox .prolis .ind_proList .item{padding: 0;border-bottom: 0;}
	.prolisbox .prolis .ind_proList .item .ll{width: 192rpx; height: 192rpx;}
	.prolisbox .prolis .ind_proList .item .rr{width: 66%; height: 192rpx;}
</style>
