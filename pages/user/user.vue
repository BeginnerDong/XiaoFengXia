<template>
	<view>
		
		<view class="userHead flexRowBetween"  @click="Router.navigateTo({route:{path:'/pages/user_perInfor/user_perInfor'}})">
			<view class="left flex">
				<view class="photo">
					<open-data type="userAvatarUrl"></open-data>
				</view>
				<view class="infor" >
					<view class="font14"><open-data type="userNickName"></open-data></view>
					<view class="lable">会员V{{mainData.level?mainData.level:''}}</view>
				</view>
			</view>
			<view class="arrow">
				<image class="arrowR" src="../../static/images/home-icon3.png" ></image>
			</view>
		</view>
		
		<view class="f5H10"></view>
		<view class="XlineNav">
			<view class="info" @click="Router.navigateTo({route:{path:'/pages/myOrder/myOrder'}})">
				<view class="ilblock listimg">
					<image src="../../static/images/about-icon1.png"></image>
				</view>
				<view class="ilblock">我的订单</view>
				<image class="arrowR" src="../../static/images/home-icon3.png" ></image>
			</view>
			<view class="info" @click="Router.navigateTo({route:{path:'/pages/myIntegral/myIntegral'}})">
				<view class="ilblock listimg">
					<image src="../../static/images/about-icon2.png"></image>
				</view>
				<view class="ilblock">我的积分</view>
				<image class="arrowR" src="../../static/images/home-icon3.png" ></image>
			</view>
			<view class="info" @click="Router.navigateTo({route:{path:'/pages/myCoupon/myCoupon'}})">
				<view class="ilblock listimg">
					<image src="../../static/images/about-icon3.png"></image>
				</view>
				<view class="ilblock">我的优惠券</view>
				<image class="arrowR" src="../../static/images/home-icon3.png" ></image>
			</view>
			<view class="info" @click="Router.navigateTo({route:{path:'/pages/myYongJin/myYongJin'}})">
				<view class="ilblock listimg">
					<image src="../../static/images/about-icon4.png"></image>
				</view>
				<view class="ilblock">我的佣金</view>
				<image class="arrowR" src="../../static/images/home-icon3.png" ></image>
			</view>
			<view class="info" @click="Router.navigateTo({route:{path:'/pages/myAddress/myAddress'}})">
				<view class="ilblock listimg">
					<image src="../../static/images/about-icon5.png"></image>
				</view>
				<view class="ilblock">我的地址</view>
				<image class="arrowR" src="../../static/images/home-icon3.png" ></image>
			</view>
			<view class="info" @click="Router.navigateTo({route:{path:'/pages/myCommunityCenter/myCommunityCenter'}})">
				<view class="ilblock listimg">
					<image src="../../static/images/about-icon6.png"></image>
				</view>
				<view class="ilblock">社区中心</view>
				<image class="arrowR" src="../../static/images/home-icon3.png" ></image>
			</view>
			
			<view class="f5H10"></view>
			<view class="info" @click="Router.navigateTo({route:{path:'/pages/honeycomb_login/honeycomb_login'}})">
				<view class="ilblock listimg">
					<image src="../../static/images/about-icon7.png"></image>
				</view>
				<view class="ilblock">蜂窝中心</view>
				<image class="arrowR" src="../../static/images/home-icon3.png" ></image>
			</view>
			<view class="info" @click="into">
				<view class="ilblock listimg">
					<image src="../../static/images/about-icon8.png"></image>
				</view>
				<view class="ilblock">蜂客申请入口</view>
				<image class="arrowR" src="../../static/images/home-icon3.png" ></image>
			</view>
			
			<view class="f5H10"></view>
			<button class="info" open-type="contact" style="border-radius:0;line-height:0;background: #fff;color: #000000;">
				<view class="ilblock listimg">
					<image src="../../static/images/about-icon9.png"></image>
				</view>
				<view class="ilblock">在线客服</view>
				<image class="arrowR" src="../../static/images/home-icon3.png" ></image>
			</button>
		</view>
		<view style="height: 40rpx;width: 100%;background: #f5f5f5;"></view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:{}
			}
		},
		
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		
		onShow() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		onPullDownRefresh() {
			console.log('refresh');
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
			setTimeout(function() {
				uni.stopPullDownRefresh();
			}, 1000);
		},
		
		methods: {
			
			formIdAdd(e) {
				const self = this;
				console.log(e)
				self.$apis.WxFormIdAdd(e.detail.formId, Date.parse(new Date()) / 1000 + 7 * 86400);
			},
			
			into(){
				const self = this;
				if(uni.getStorageSync('user_info').info.check_status==0){
					self.Router.navigateTo({route:{path:'/pages/myShopApply/myShopApply'}})
				}else if(uni.getStorageSync('user_info').info.check_status==1){
					self.$Utils.showToast('您的资料审核中', 'none');
				}else if(uni.getStorageSync('user_info').info.check_status==2&&uni.getStorageSync('user_info').info.level>=3){
					self.Router.navigateTo({route:{path:'/pages/myShop/myShop'}})
				}
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
		},
	};
</script>

<style>
	@import "../../assets/style/user.css";

</style>
