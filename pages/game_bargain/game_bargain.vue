<template>
	<view>
		
		<view class="pdlr4">
			<view class="ind_proList">
				<view class="item flexRowBetween"  >
					<view class="ll" @click="Router.navigateTo({route:{path:'/pages/game_bargainDetail/game_bargainDetail'}})">
						<image src="../../static/images/home-img7.png" mode=""></image>
					</view>
					<view class="rr">
						<view class="avoidOverflow2 title" @click="Router.navigateTo({route:{path:'/pages/game_bargainDetail/game_bargainDetail'}})">40枚正宗土鸡蛋农家散养新鲜农村野外放养笨鸡蛋草鸡蛋柴鸡蛋</view>
						<view class="money flexRowBetween">
							<view class="flex left">
								<view class="price">26.8</view>
								<view class="yuan price">36</view>
							</view>
							<view class="addbtn" @click="Router.navigateTo({route:{path:'/pages/game_bargainDetailTwo/game_bargainDetailTwo'}})">砍价</view>
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
				showView: false,
				Router:this.$Router,
				score:'',
				wx_info:{},
				scrollTop: 100,
				index: 0
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
