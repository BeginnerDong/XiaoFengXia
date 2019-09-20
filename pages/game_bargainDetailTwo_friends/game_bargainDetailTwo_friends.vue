<template>
	<view>
		<view class="detail_join" style="padding:20rpx 4%">
			<view class="twolist">
				<view class="item flexRowBetween" v-for="(item,index) in friendsList" :key="index">
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
		
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				webSelf: this,
				showView: false,
				is_show:false,
				show_addok:false,
				friendsList:[
					{},{},{},{},{},{},{}
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
			showAddok(){
				const self = this;
				self.show_addok = !self.show_addok
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
