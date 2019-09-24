<template>
	<view>
		
		<view class="pdlr4">
			<view class="ind_proList">
				<view class="item flexRowBetween" v-for="(item,index) in indProDate" :key="index"  @click="Router.navigateTo({route:{path:'/pages/routine_openDetailDetail/routine_openDetailDetail'}})">
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
				Router:this.$Router,
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
	.ind-cont4{padding-top: 50rpx;}
	.ind-cont4 .list{ white-space: nowrap;}
	.ind-cont4 .item{width: 25%;text-align: center;font-size: 26rpx;padding-bottom:50rpx;box-sizing: border-box; display: inline-block;}
	.ind-cont4 .item image{width: 60rpx;height: 60rpx;display: block;margin: 0 auto 20rpx auto;}
	
</style>
