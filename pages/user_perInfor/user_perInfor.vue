<template>
	<view class="myRowBetween">
		<view class="item flexRowBetween">
			<view class="left">头像</view>
			<view class="right" style="border-radius:50%;width: 100rpx;height: 100rpx;overflow: hidden;">
				<open-data type="userAvatarUrl"></open-data>
			</view>
		</view>
		<view class="item flexRowBetween">
			<view class="left">昵称</view>
			<view class="right"><open-data type="userNickName"></open-data></view>
		</view>
		<view class="item flexRowBetween">
			<view class="name">会员级别</view>
			<view class="rr">V{{mainData.level?mainData.level:''}}</view>
		</view>
		<view class="item flexRowBetween">
			<view class="name">用户编号</view>
			<view class="rr">{{mainData.user_no?mainData.user_no:''}}</view>
		</view>
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
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
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
	page{background: #f5f5f5;}
</style>

 
