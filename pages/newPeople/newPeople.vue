<template>
	<view>
		
		<view class="newPeopleNav">
				<view class="item" v-for="(item,index) in newPeopleNav" :key="index"  :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/newPeople_classify/newPeople_classify?id='+$event.currentTarget.dataset.id}})">
					<image :src="item.iconUrl" mode=""></image>
					<view>{{item.name}}</view>
				</view>
		</view>
		<view class="f5H10"></view>
		<view class="infor-title">
			<view class="tt">全部商品</view>
		</view>
		<view class="pdlr4" v-if="mainData.length>0" >
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
				Router:this.$Router,
				
				mainData:[],
				newPeopleNav:[
					{iconUrl:"../../static/images/icon1.png",name:"鲜果",id:5},
					{iconUrl:"../../static/images/icon2.png",name:"海鲜",id:6},
					{iconUrl:"../../static/images/icon3.png",name:"生鲜",id:7},
					{iconUrl:"../../static/images/icon4.png",name:"零食",id:8},
					{iconUrl:"../../static/images/icon5.png",name:"坚果",id:9},
					{iconUrl:"../../static/images/icon6.png",name:"特产",id:10},
					{iconUrl:"../../static/images/icon7.png",name:"冷鲜",id:11},
					{iconUrl:"../../static/images/icon8.png",name:"礼盒",id:12}
				],
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
					type:1,
					level:0
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

	
</style>
