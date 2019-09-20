<template>
	<view>
		<view class="myExtendTop">
			<view class="bigNum">4665</view>
			<view class="yuan">总积分</view>
		</view>
		
		<view class="jifenTwoNum flexRowBetween boxShaow radius10">
			<view class="item">
				<view class="num">262</view>
				<view>可兑换积分</view>
			</view>
			<view class="item">
				<view class="num">3540</view>
				<view>已兑换积分</view>
			</view>
		</view>
		
		<view class="myRowBetween" >
			<view class="item flexRowBetween" v-for="(item,index) in rewardData" :key="index">
				<view class="left">
					<view class="avoidOverflow">商品名称商品名称商品名称商品名称商品名称商品名称</view>
					<view class="time">2019-09-19</view>
				</view>
				<view class="right">-223</view>
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
				]
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
	.myRowBetween .item .left{width: 70%;}
	.myRowBetween .item .right{width: 30%;}
	
</style>
