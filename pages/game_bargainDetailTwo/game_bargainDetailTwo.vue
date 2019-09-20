<template>
	<view>
		
		<view class="detailxqBan">
			<image src="../../static/images/details-img1.png" mode=""></image>
		</view>
		
		<view class="detail_one pr">
			<view class="">
				<view class="price font14">56.5</view>
			</view>
			<view class="flexRowAround shareBtn" style="justify-content: flex-end;">
				<image src="../../static/images/beestoenjoy-icon1.png" mode=""></image>
				<view class="" style="">分享</view>
			</view>
			<view class="flexRowBetween color2 font13 number">
				<view class="">库存：2133</view>
				<view class="">销量：562</view>
			</view>
			<view class="tit font14">梨子新鲜2件带项10斤陕西早酥梨当季梨皇冠香梨应季水果包邮</view>
		</view>
		<view class="f5H10"></view>
		<view class="detail_join">
			<view class="flexRowBetween">
				<view class="font14">好友砍价</view>
				<view class="font13 color3" @click="webSelf.$Router.navigateTo({route:{path:'/pages/game_bargainDetailTwo_friends/game_bargainDetailTwo_friends'}})">查看更多&gt;</view>
			</view>
			<view class="twolist">
				<view class="item flexRowBetween">
					<view class="ll flex">
						<view class="photo">
							<image src="../../static/images/about-img1.png" mode=""></image>
						</view>
						<view class="tit avoidOverflow">名称名称名称</view>
					</view>
					<view class="rr red">-5元</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll flex">
						<view class="photo">
							<image src="../../static/images/about-img1.png" mode=""></image>
						</view>
						<view class="tit avoidOverflow">名称名称名称</view>
					</view>
					<view class="rr red">-5元</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll flex">
						<view class="photo">
							<image src="../../static/images/about-img1.png" mode=""></image>
						</view>
						<view class="tit avoidOverflow">名称名称名称</view>
					</view>
					<view class="rr red">-5元</view>
				</view>
			</view>
		</view>
		<view class="f5H10"></view>
		<view class="infor-title">
			<view class="tt">详情介绍</view>
		</view>
		<view class="detailTexCont">
			<view class="center">
				<view>详情介绍详情介绍详情介绍详情介绍详情</view>
				<view>详情介绍详情介绍详情介绍详情</view>
				<view>详情介绍详情介绍详情介绍详情介绍详情介绍详情</view>
				<view>详情介绍详情介绍详情介绍</view>
				<view>
					<image src="../../static/images/details-img2.png" mode="widthFix"></image>
				</view>
				<view>详情介绍详情介绍详情介绍详情介绍详情</view>
				<view>详情介绍详情介绍详情介绍详情</view>
				<view>详情介绍详情介绍详情介绍详情介绍详情介绍详情</view>
				<view>详情介绍详情介绍详情介绍</view>
				<view>
					<image src="../../static/images/details-img3.png" mode="widthFix"></image>
				</view>
			</view>
		</view>
		
		<!-- 底部菜单按钮 -->
		<view class="xqbotomBar">
			<view class="left flexRowBetween">
				<view class="ite" style="width:100%" @click="prev">
					<image src="../../static/images/details=icon1.png" mode=""></image>
					<view>返回</view>
				</view>
			</view>
			<view class="payBtn flexRowBetween">
				<view class="item hei" style="width: 100%;">
					<view class="price">586</view>
				</view>
			</view>
		</view>
		
		<!-- 分享出去帮好友砍价按钮 -->
		<view class="xqbotomBar" style="bottom: 130rpx;">
			<view class="left flexRowBetween">
				<view class="ite" style="width:100%;" @click="prev">
					<image src="../../static/images/details=icon1.png" mode=""></image>
					<view>返回</view>
				</view>
			</view>
			<view class="payBtn flexRowBetween">
				<view class="item hei" style="width: 100%;" @click="goKanJia">
					<view class="pubColor">帮好友砍价</view>
				</view>
			</view>
		</view>
		
		<!-- 分享出去已帮好友砍过价 -->
		<view class="xqbotomBar" style="bottom: 260rpx;">
			<view class="left flexRowBetween">
				<view class="ite" style="width:100%;" @click="prev">
					<image src="../../static/images/details=icon1.png" mode=""></image>
					<view>返回</view>
				</view>
			</view>
			<view class="payBtn flexRowBetween">
				<view class="item" style="width: 100%; background: #ddd;" @click="webSelf.$Router.navigateTo({route:{path:'/pages/confirmOrder/confirmOrder'}})">
					<view class="color3">已砍价</view>
				</view>
			</view>
		</view>
		
		<!-- 砍价弹框 -->
		<view class="PinDanAlert" v-if="is_show">
			<view class="explain center" style="padding: 60rpx;">
				<view class="colseBtn" @click="goKanJia" style="left: auto;right: 10rpx;top: 20rpx;">×</view>
				<view class="tit">帮好友成功砍掉<text class="red">56元</text></view>
				
				<view class="submitbtn">
					<button type="button" style="width: 100%; margin:20rpx 0;" @click="goKanJia">确定</button>
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
				is_show:false,
				show_addok:false
			}
		},
		
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			goKanJia(){
				const self = this;
				self.is_show = !self.is_show
			},
			prev(){
				const self = this;
				self.$router.go(-1)
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
	page{padding-bottom: 140rpx;}
	
	
	.payBtn .hei .price::before{content: "总计：";}
	.shareBtn{font-size: 24rpx;padding: 0 20rpx;border-radius:30rpx 0 0 30rpx; line-height:46rpx;background: #eee; color: #666; margin-left: 6rpx;position: absolute; right: 0;top: 20rpx;}
	.shareBtn image{width: 26rpx; height: 29rpx;display: block;margin-right: 6rpx;}
</style>
