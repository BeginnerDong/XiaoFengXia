<template>
	<view>
		<view class="myExtendTop">
			<view class="bigNum">4665</view>
			<view class="yuan">总积分</view>
			<view class="txBtn"  @click="Router.navigateTo({route:{path:'/pages/myCashOut/myCashOut'}})">提现</view>
		</view>
		
		<view class="myRowBetween" >
			<view class="item flexRowBetween" v-for="(item,index) in rewardData" :key="index">
				<view class="left">
					<view class="avoidOverflow">提现</view>
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
				Router:this.$Router,
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

				self.$apis.orderGet(postData, callback);

			},

		},
	};
</script>

<style>
	@import "../../assets/style/user.css";
	
</style>
