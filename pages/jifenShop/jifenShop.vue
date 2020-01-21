<template>
	<view>
		<view class="index_topBj pdlr4">
			<view class="banner-box" style="padding-top: 30rpx 0;">
				<view class="banner radius10 oh">
					<swiper class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000" indicator-active-color="#434343">
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
		<view class="ind_cont5">
			<view class="flexRowBetween">
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/coupon/coupon'}})">
					<image src="../../static/images/mall-icon1.png" mode=""></image>
					<view class="tit">优惠券</view>
				</view>
				<view class="item"  @click="Router.navigateTo({route:{path:'/pages/game/game'}})">
					<image src="../../static/images/mall-icon2.png" mode=""></image>
					<view class="tit">游戏</view>
				</view>
			</view>
		</view>
		
		<view class="f5H10"></view>
		<view class="infor-title flexRowBetween">
			<view class="tt">精选推荐</view>
		</view>
		<view class="proLis flexRowBetween">
			<view class="item-lis" v-for="(item,index) in mainData" :key="index" :data-id="item.id"
			@click="Router.navigateTo({route:{path:'/pages/jiFenShopDetail/jiFenShopDetail?id='+$event.currentTarget.dataset.id}})">
				<image class="img" :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" alt="" />
				<view class="tit avoidOverflow">{{item.title}}</view>
				<view class="price">{{item.price}}<text>/积分:{{item.score}}</text></view>
			</view>
		</view>
		
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				swiperData:{},
				mainData:[]
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getSliderData','getMainData'], self);
		},
		
		onPullDownRefresh() {
			console.log('refresh');
			const self = this;
			self.getMainData(true);
			setTimeout(function() {
				uni.stopPullDownRefresh();
			}, 1000);
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
					category_id:19,
					type:2
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
			
			getSliderData() {
				const self = this;		
				const postData = {};
				postData.searchItem = {
					thirdapp_id:2,	
					title:'积分商城轮播'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.swiperData = res.info.data[0]
					}
					console.log('self.swiperData',self.swiperData)
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
		padding-bottom: 60rpx;
	}
	.ind_cont5{padding: 30rpx 4%;}
	.ind_cont5 .item{width: 330rpx;height: 200rpx;overflow: hidden;position: relative;border-radius: 8rpx;}
	.ind_cont5 .item image{width: 100%;height: 100%;}
	.ind_cont5 .item .tit{position: absolute;top: 36rpx;left: 26rpx;color: #fff;font-size:36rpx;}
	
	.proLis{padding: 20rpx 4%;flex-wrap: wrap;}
	.proLis .item-lis{ width: 330rpx; overflow: hidden;background: #fff;margin: 15rpx 0; position: relative;border-radius: 10rpx 10rpx 0 0;}
	.proLis .item-lis .img{width: 100%;height: 330rpx;display: block;margin: 0 auto;}
	.proLis .item-lis .tit{ font-size: 26rpx; padding: 20rpx 4% 10rpx 4%; line-height: 32rpx;}
	.proLis .item-lis .price{font-size: 28rpx;padding: 0 4%; color: #ff3b3b;}
	.proLis .item-lis .price:before{content: "￥"; font-size: 26rpx;margin-right:4rpx;}
</style>
