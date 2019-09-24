<template>
	<view>
		<view class="orderNav orderNavThree">
			<view class="tt" :class="current==1?'on':''" @click="change('1')">待使用</view>
			<view class="tt" :class="current==2?'on':''" @click="change('2')">已使用</view>
			<view class="tt" :class="current==3?'on':''" @click="change('3')">已过期</view>
		</view>
		
		<view class="cuponList" v-if="current==1">
			<view class="item" v-for="(item,index) in cuponList" :key="index" >
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
				<view class="rrTex">待使用</view>
			</view>
		</view>
		<view class="cuponList" v-if="current==2">
			<view class="item" v-for="(item,index) in cuponList" :key="index" >
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
				<view class="rrTex">已使用</view>
				<view class="fixIcon"><image src="../../static/images/coupons-icon3.png" mode=""></image></view>
			</view>
		</view>
		<view class="cuponList" v-if="current==3">
			<view class="item" v-for="(item,index) in cuponList" :key="index" >
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
				<view class="rrTex">已过期</view>
				<view class="fixIcon"><image src="../../static/images/coupons-icon2.png" mode=""></image></view>
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
				score: '',
				wx_info: {},
				current:1,
				cuponList:[
					{},{},{}
				]
			}
		},

		methods: {
			change(current) {
				const self = this;
				if(current!=self.current){
					self.current = current
				}
			},
			getMainData() {
				const self = this;
				self.$apis.userGet(postData, callback);
			}
		}
	}
</script>

<style>
	@import "../../assets/style/page.css";
	page{ background: #f5f5f5;padding-bottom: 80rpx;}

	.orderNavThree .tt{ width: 33.3%;}
</style>
