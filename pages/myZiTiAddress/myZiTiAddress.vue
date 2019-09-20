<template>
	<view>
		
		<view class="myRowBetween" >
			<view class="item flexRowBetween" v-for="(item,index) in rewardData"  @click="onTabsItemTap">
				<view class="left">
					<view class="flex">
						<view>中华世纪智能柜机</view>
						<view class="font12 color3 iphone">15989566656</view>
					</view>
					<view class="time">陕西省西安市雁塔区高新科技路西段239号中华世纪城</view>
				</view>
				<view class="right">
					<image class="setIcon" :src="curr==index?'../../static/images/address-icon3.png':'../../static/images/address-icon4.png'" mode="" ></image>
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
				score:'',
				wx_info:{},
				rewardData:[
					{},{},{}
				],
				curr:0
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
		
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';

				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data;
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getMainData');

				};
				self.$apis.orderGet(postData, callback);

			},

		},
	};
</script>

<style>
	@import "../../assets/style/user.css";
	.myRowBetween .item .left{width: 90%;}
	.myRowBetween .item .right{width: 5%;}
	.setIcon{padding: 20rpx 0; width: 30rpx; height: 30rpx; display: block;}
	.iphone{margin-left: 20rpx;}
	
</style>
