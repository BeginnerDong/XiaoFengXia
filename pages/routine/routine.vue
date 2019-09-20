<template>
	<view>
		
		<view class="newPeopleNav">
				<view class="item" v-for="(item,index) in newPeopleNav" :key="index"  @click="webSelf.$Router.navigateTo({route:{path:'/pages/routine_classify/routine_classify'}})">
					<image :src="item.iconUrl" mode=""></image>
					<view>{{item.name}}</view>
				</view>
		</view>
		<view class="f5H10"></view>
		<view class="infor-title">
			<view class="tt">全部商品</view>
		</view>
		<view class="pdlr4">
			<view class="ind_proList">
				<view class="item flexRowBetween" @click="webSelf.$Router.navigateTo({route:{path:'/pages/goodsJoinDetail/goodsJoinDetail'}})">
					<view class="ll">
						<image src="../../static/images/home-img7.png" mode=""></image>
					</view>
					<view class="rr">
						<view class="avoidOverflow2 title">40枚正宗土鸡蛋农家散养新鲜农村野外放养笨鸡蛋草鸡蛋柴鸡蛋</view>
						<view class="lable">
							<view class="tt">5积分</view>
							<view class="tt">2人团</view>
						</view>
						<view class="money flexRowBetween">
							<view class="flex left">
								<view class="price">26.8</view>
								<view class="yuan price">36</view>
							</view>
							<view class="addbtn">参团</view>
						</view>
					</view>
				</view>
				<view class="item flexRowBetween" v-for="(item,index) in indProDate" :key="index"  @click="webSelf.$Router.navigateTo({route:{path:'/pages/routine_openDetailDetail/routine_openDetailDetail'}})">
					<view class="ll">
						<image src="../../static/images/home-img7.png" mode=""></image>
					</view>
					<view class="rr">
						<view class="avoidOverflow2 title">40枚正宗土鸡蛋农家散养新鲜农村野外放养笨鸡蛋草鸡蛋柴鸡蛋</view>
						<view class="lable">
							<view class="tt">5积分</view>
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
				newPeopleNav:[
					{iconUrl:"../../static/images/icon1.png",name:"鲜果"},
					{iconUrl:"../../static/images/icon2.png",name:"海鲜"},
					{iconUrl:"../../static/images/icon3.png",name:"生鲜"},
					{iconUrl:"../../static/images/icon4.png",name:"零食"},
					{iconUrl:"../../static/images/icon5.png",name:"坚果"},
					{iconUrl:"../../static/images/icon6.png",name:"特产"},
					{iconUrl:"../../static/images/icon7.png",name:"冷鲜"},
					{iconUrl:"../../static/images/icon8.png",name:"礼盒"}
				],
				indProDate:[
					{},{},{},{},{}
				]
			}
		},
		
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
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

	page {
		padding-bottom: 60rpx;
	}

	
</style>
