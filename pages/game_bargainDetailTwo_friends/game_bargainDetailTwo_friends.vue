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
				Router:this.$Router,
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
	page{padding-bottom: 80rpx;}
	
</style>
