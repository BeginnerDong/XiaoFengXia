<template>
	<view>
		
		<view class="detail_join" style="padding: 0rpx 4%;">
			<view class="twolist">
				<view class="item flexRowBetween" v-for="(item,index) in pintuanList" :key="index">
					<view class="ll flex">
						<view class="photo">
							<image src="../../static/images/about-img1.png" mode=""></image>
						</view>
						<view class="tit avoidOverflow">名称名称名称</view>
					</view>
					<view class="rr flexRowBetween">
						<view class="downTime">
							<view class="red font14">还差1人拼成</view>
							<view class="time">剩余23:23:48</view>
						</view>
						<view class="go" @click="goPinDan">去拼团</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="PinDanAlert" v-if="is_show">
			<view class="explain center">
				<view class="colseBtn" @click="goPinDan" style="left: auto;right: 10rpx;top: 20rpx;">×</view>
				<view class="tit">参与名称的拼单</view>
				<view><text class="red">还剩1个名额</text>。23:23:56后结束</view>
				<view class="flexCenter">
					<view class="photo">
						<image src="../../static/images/beestoenjoy-img1.png"></image>
					</view>
					<view class="photo noPic">
						<image src="../../static/images/who.png"></image>
					</view>
				</view>
				<view class="submitbtn">
					<button type="button" style="width: 100%; margin:20rpx 0;" @click="webSelf.$Router.navigateTo({route:{path:'/pages/confirmOrder/confirmOrder'}})">去拼单</button>
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
				pintuanList:[
					{},{},{},{},{},{},{},{}
				]
			}
		},
		
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			goPinDan(){
				const self = this;
				self.is_show = !self.is_show
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

	page {
		padding-bottom: 140rpx;
	}
	

</style>
