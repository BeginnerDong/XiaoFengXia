<template>
	<view>
		
		<view class="detailxqBan">
			<image src="../../static/images/details-img1.png" mode=""></image>
		</view>
		
		<view class="detail_one">
			<view class="flex">
				<view class="price font14">56.5</view>
				<view class="" style="font-size: 24rpx;padding: 0 10rpx;border-radius: 20rpx; line-height: 36rpx;background: #eee; color: #666; margin-left: 6rpx;">积分可抵用</view>
			</view>
			<view class="flexRowBetween color2 font13 number">
				<view class="">库存：2133</view>
				<view class="">销量：562</view>
			</view>
			<view class="tit font14">梨子新鲜2件带项10斤陕西早酥梨当季梨皇冠香梨应季水果包邮</view>
		</view>
		<view class="f5H10"></view>
		<view class="detal_guige pdlr4 flexRowBetween"  @click="showSel">
			<view class="flexRowAround ll">
				<view>规格</view>
				<view class="yyDate-lis flex">
					<view>10斤装</view>
					<view>10斤装</view>
					<view>10斤装</view>
				</view>
			</view>
			<view class="rr">
				<image class="arrowR" src="../../static/images/home-icon3.png" mode=""></image>
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
				<view class="ite" @click="prev">
					<image src="../../static/images/details=icon1.png" mode=""></image>
					<view>返回</view>
				</view>
			</view>
			<view class="payBtn flexRowBetween">
				<view class="item hei" style="width: 100%;" @click="Router.navigateTo({route:{path:'/pages/confirmOrderTwo/confirmOrderTwo'}})">
					<view class="price">586</view>
					<view class="name">结算</view>
				</view>
			</view>
		</view>
		
		<!-- 选择规格弹框 -->
		<view class="showSel" v-if="is_show" >
			<view class="showSelCont fix">
				<view class="colseBtn" @click="showSel">×</view>
				<view class="ind_proList">
					<view class="item flexRowBetween" style="padding:0rpx 0 20rpx 0;border-bottom: 0;">
						<view class="ll">
							<image src="../../static/images/home-img7.png" mode=""></image>
						</view>
						<view class="rr">
							<view class="avoidOverflow2 title">40枚正宗土鸡蛋农家散养新鲜农村野外放养笨鸡蛋草鸡蛋柴鸡蛋</view>
							<view class="money">
								<view class="flex left">
									<view class="price">26.8</view>
								</view>
							</view>
							
						</view>
					</view>
				</view>
				<view class="yy-title" style="padding: 10px 0;">规格</view>
				<view class="yyDate-lis flexRowBetween" style="flex-wrap: wrap; justify-content: flex-start;">
					<view v-for="(item,index) in seltSpecsList" :key="index" :class="[selCurrent == index? 'on':'']" @click="seltSpecs(index)">{{item}}</view>
				</view>
				<view class="flexRowBetween" style="padding: 30rpx 0; border-top: 2rpx solid #e7e7e7; border-bottom: 2rpx solid #e7e7e7; margin-top: 10rpx;">
					<view class="yy-title">购买数量</view>
					<view class="numBox" style="position: relative; right: auto; bottom: auto;">
						<view>+</view>
						<view class="num">1</view>
						<view>-</view>
					</view>
				</view>
				<view class="submitbtn" style="margin-top: 60rpx;">
					<button class="hei" style="margin-bottom: 0;" type="submit" @click="Router.navigateTo({route:{path:'/pages/confirmOrder/confirmOrder'}})" >确定</button>
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
				is_show:false,
				show_addok:false,
				selCurrent:0,
				seltSpecsList:['10斤装','10斤装','10斤装','10斤装','10斤装']
			}
		},
		
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			showSel(){
				const self = this;
				self.is_show = !self.is_show;
			},
			prev(){
				const self = this;
				self.$router.go(-1)
			},
			// 商品属性
			seltSpecs(index){
				const self = this;
				self .selCurrent=index
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
	@import "../../assets/style/car.css";
	page{padding-bottom: 140rpx;}
	.payBtn .hei .price::before{content: "积分：";}
	
	.showSel {width: 100%;height: 100%;background: rgba(0, 0, 0, 0.5);position: fixed;top: 0;right: 0;bottom: 0;left: 0;z-index: 10000;}
	.showSelCont {padding:40rpx 4%;background: #fff;}
	.showSelCont.fix{position: fixed;bottom: 0;left: 0;box-sizing: border-box;width: 100%;border-radius: 20rpx 20rpx 0 0;}
	.yy-title {font-size: 28rpx;}
	.yyDate-lis {max-height: 270rpx; overflow-y: auto;}
	.yyDate-lis view {width: 130rpx;height: 56rpx;font-size: 26rpx;color: #666;text-align: center;line-height: 56rpx;background: #f5f5f5;margin: 0 20rpx 30rpx 20rpx;}
	.yyDate-lis view.on{background: #F3D012; color: #333;}
	.showSel .numBox view {background: #f5f5f5;}
	.detal_guige{padding: 20rpx 4%;}
	.detal_guige .ll{width: 90%; display: flex; justify-content: flex-start;align-items: center;}
	.detal_guige .rr{width: 10%;display: flex;justify-content: flex-end;}
	.detal_guige .yyDate-lis view{margin:0 0 0 20rpx;}
</style>
