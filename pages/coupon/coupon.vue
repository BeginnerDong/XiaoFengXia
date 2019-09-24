<template>
	<view>
		<view class="index_topBj">
			<view class="banner-box">
				<view class="banner radius10 oh">
					<swiper class="swiper-box" indicator-dots="indicatorDots" autoplay="autoplay" interval="interval" duration="duration" indicator-active-color="#434343">
						<block v-for="(item,index) in labelData" :key="index">
							<swiper-item class="swiper-item">
								<image :src="item" class="slide-image" />
							</swiper-item>
						</block>
					</swiper>
				</view>
			</view>
		</view>
		<view class="f5H10"></view>
		<view class="infor-title flexRowBetween" style="padding: 30rpx 4%;background: #fff;">
			<view class="tt" style="font-size: 28rpx;">积分兑换优惠券</view>
			<view class="more" @click="Router.navigateTo({route:{path:'/pages/coupon_rule/coupon_rule'}})">兑换规则&gt;</view>
		</view>
		
		<view class="orderNav" style="background: #f5f5f5;">
			<view class="tt" :class="current==1?'on':''" @click="change('1')">积分兑换</view>
			<view class="tt" :class="current==2?'on':''" @click="change('2')">免费领取</view>
		</view>
		<view class="cuponList" v-if="current==1">
			<view class="item" v-for="(item,index) in cuponList" :key="index"  @click="deltAlert">
				<view class="infor">
					<view class="flex">
						<view class="left flexCenter">
							<view class="mny">20</view>
						</view>
						<view class="text">
							<view>牛肉生鲜</view>
							<view>200积分兑换</view>
							<view>满100使用</view>
						</view>
					</view>
					<view class="data">2019.09.19-2019.09-30</view>
				</view>
				<view class="rrTex">立即兑换</view>
			</view>
		</view>
		<view class="cuponList" v-if="current==2">
			<view class="item" v-for="(item,index) in freeList" :key="index"  @click="blackAlert">
				<view class="infor">
					<view class="flex">
						<view class="left flexCenter">
							<view class="mny">20</view>
						</view>
						<view class="text">
							<view>牛肉生鲜</view>
							<view style="margin-top: 30rpx;">满100使用</view>
						</view>
					</view>
					<view class="data">2019.09.19-2019.09-30</view>
				</view>
				<view class="rrTex">立即领取</view>
			</view>
		</view>
		
		<!-- 兑换弹框 -->
		<view class="xieyiAlert" v-if="is_show">
			<view class="infor center" style="padding: 120rpx 30px;height: auto;border-radius: 10rpx;">
				<view class="colseBtn"  @click="deltAlert" style="top: 20rpx;right: 20rpx;left:auto;">×</view>
				<view class="tit font16" style="padding-bottom: 60rpx;">确认是否删除这个技能</view>
				<view class="btnB flexRowBetween">
					<view class="on" >是</view>
					<view @click="deltAlert">否</view>
				</view>
			</view>
		</view>
		
		<!-- 免费领取 -->
		
		<view class="blackAlert radius10 center" v-if="is_blackAlert">
			<view class="colseBtn"  @click="blackAlert" style="top: 20rpx;right: 20rpx;left:auto;">×</view>
			<view class="tit">领取成功，可以在我的优惠券查看</view>
			<view class="okBtn" @click="blackAlert">确定</view>
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
				current:1,
				selt:1,
				is_show:false,
				is_blackAlert:false,
				labelData: [
					"../../static/images/home-banner.png",
					"../../static/images/home-banner.png",
				],
				cuponList:[
					{},{},{},{}
				],
				freeList:[
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
			chageSelt(selt){
				const self = this;
				if(selt!=self.selt){
					self.selt=selt
				}
			},
			deltAlert(){
				const self = this;
				self.is_show=!self.is_show;
			},
			blackAlert(){
				const self = this;
				self.is_blackAlert=!self.is_blackAlert;
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
	page { background: #f5f5f5;padding-bottom: 60rpx;}
	
	.blackAlert{ position: fixed; top: 50%;left: 50%;transform: translate(-50%,-50%); width: 80%;background: rgba(0,0,0,0.5); color: #fff; padding: 90rpx 3%;box-sizing: border-box; z-index: 30;}
	.blackAlert .colseBtn{width:40rpx; height: 40rpx; line-height: 32rpx;background: none;border:2rpx solid #fff; color: #fff;font-size: 36rpx; line-height: 36rpx;}
	.blackAlert .tit{font-size:28rpx; margin-bottom:80rpx;}
	.blackAlert .okBtn{ width: 140RPX; height: 60rpx;line-height: 60rpx; color: #222; border-radius: 8rpx; text-align: center;background: #F3D012;margin: 0 auto;}
	

</style>
