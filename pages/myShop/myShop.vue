<template>
	<view>
		<view class="index_topBj">
			<view class="banner-box">
				<view class="banner radius10 oh">
					<swiper class="swiper-box" indicator-dots="indicatorDots" autoplay="autoplay" interval="interval" duration="duration" indicator-active-color="#434343">
						<block v-for="(item,index) in labelData" :key="index">
							<swiper-item class="swiper-item" @click="webSelf.$Router.navigateTo({route:{path:'/pages/simpleDetail/simpleDetail'}})">
								<image :src="item" class="slide-image" />
							</swiper-item>
						</block>
					</swiper>
				</view>
			</view>
		</view>
		
		<view class="f5H10"></view>
		
		<view class="orderNav">
			<view class="tt" :class="current==1?'on':''" @click="change('1')">尝鲜区</view>
			<view class="tt" :class="current==2?'on':''" @click="change('2')">全部商品</view>
		</view>
		<view class="pdlr4">
			
			<view class="ind_proList" v-if="current==1">
				<view class="item flexRowBetween" v-for="(item,index) in indProDate" :key="index" @click="webSelf.$Router.navigateTo({route:{path:'/pages/myShopPayDetail/myShopPayDetail'}})">
					<view class="ll">
						<image src="../../static/images/home-img7.png" mode=""></image>
					</view>
					<view class="rr">
						<view class="avoidOverflow2 title">40枚正宗土鸡蛋农家散养新鲜农村野外放养笨鸡蛋草鸡蛋柴鸡蛋</view>
						<view class="lable">
							<view class="tt">5积分</view>
						</view>
						<view class="money flexRowBetween">
							<view class="flex left">
								<view class="price">26.8</view>
								<view class="yuan price">36</view>
							</view>
							<view class="addbtn">购买</view>
						</view>
						
					</view>
				</view>
			</view>
			
			<view class="ind_proList" v-if="current==2">
				<view class="item flexRowBetween" v-for="(item,index) in indProDate" :key="index" @click="webSelf.$Router.navigateTo({route:{path:'/pages/goodsOpenDetail/goodsOpenDetail'}})">
					<view class="ll">
						<image src="../../static/images/home-img7.png" mode=""></image>
					</view>
					<view class="rr">
						<view class="avoidOverflow2 title">40枚正宗土鸡蛋农家散养新鲜农村野外放养笨鸡蛋草鸡蛋柴鸡蛋</view>
						<view class="lable">
							<view class="tt">5积分</view>
							<view class="tt">5佣金</view>
							<view class="tt">2人团</view>
						</view>
						<view class="money flexRowBetween">
							<view class="flex left">
								<view class="price">26.8</view>
								<view class="yuan price">36</view>
							</view>
							<view class="addbtn">开团</view>
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
				webSelf: this,
				showView: false,
				score:'',
				wx_info:{},
				scrollTop: 100,
				index: 0,
				current:1,
				labelData: [
					"../../static/images/home-banner.png",
					"../../static/images/home-banner.png",
				],
				indProDate:[
					{},{},{},{}
				]
			}
		},
		
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			change(current) {
				const self = this;
				if(current!=self.current){
					self.current = current
				}
			},
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';

				self.$apis.orderGet(postData, callback);
			},
		},
	};
</script>

<style>
	@import "../../assets/style/page.css";

	page {padding-bottom: 60rpx;}
</style>
