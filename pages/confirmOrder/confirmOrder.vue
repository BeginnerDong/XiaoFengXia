<template>
	<view class="" >

		<view style="background: #fff;">
			
			<view class="ind_proList">
				<view class="item flexRowBetween" v-for="(item,index) in proListDate" :key="index">
					<view class="ll">
						<image src="../../static/images/order-img1.png" mode=""></image>
					</view>
					<view class="rr">
						<view class="avoidOverflow2 title">40枚正宗土鸡蛋农家散养新鲜农村野外放养笨鸡蛋草鸡蛋柴鸡蛋</view>
						<view class="lable">
							<view class="tt">5积分</view>
							<view class="tt">2人团</view>
						</view>
						<view class="money flexRowBetween">
							<view class="flex left">
								<view class="price">56.8</view>
							</view>
							<view class="numBox" style="position: absolute; right: 0; bottom: 0;">
								<view >-</view>
								<view class="num">{{count}}</view>
								<view >+</view>
							</view>
						</view>
					</view>
				</view>
			</view>
			
			<view class="pdlr4 borderB1 flexRowBetween cfmSetAdrs">
				<view class="fonot14">优惠券</view>
				<view class="font14 color3">暂无优惠券使用</view>
			</view>
			<view class="tihuoSelt flex">
				<view :class="num==1?'on':''" @click="change('1')">提货点自提</view>
				<view :class="num==2?'on':''" @click="change('2')">快递配送</view>
			</view>
			<view class="" v-if="num==1">
				<view class="cfmSetAdrs flexRowBetween" style="align-items: normal;">
					<view class="yy-title">提货点</view>
					<view class="yy-adrs flexRowBetween">
						<view class="cont font13">
							<view class="name" >张三&nbsp;&nbsp;&nbsp;&nbsp;15689795323</view>
							<view class="" >陕西省西安市雁塔区高新大都荟</view>
						</view>
						<view class="arrow" @click="webSelf.$Router.navigateTo({route:{path:'/pages/myZiTiAddress/myZiTiAddress'}})">
							<image class=" arrowR" src="../../static/images/home-icon3.png" alt=""/>
						</view>
						
					</view>
				</view>
			</view>
			<view v-if="num==2">
				<view class="cfmSetAdrs flexRowBetween borderB1" style="align-items: normal;">
					<view class="yy-title">快递点</view>
					<view class="yy-adrs flexRowBetween">
						<view class="cont font13">
							<view class="name" >张丹&nbsp;&nbsp;&nbsp;&nbsp;15689795620</view>
							<view class="" >陕西省西安市雁塔区高新大都荟</view>
						</view>
						<view class="arrow"  @click="webSelf.$Router.navigateTo({route:{path:'/pages/myAddress/myAddress'}})">
							<image class=" arrowR" src="../../static/images/home-icon3.png" alt=""/>
						</view>
					</view>
				</view>
				<view class="cfmSetAdrs flexRowBetween">
					<view class="fonot14">运费</view>
					<view class="font14 price">10</view>
				</view>
			</view>
			
		</view>
		
		<view class="confm_BFix">
			合计： <view class="price">56.00</view>
			<view class="btn" @click="webSelf.$Utils.stopMultiClick(submit)">立即购买</view>
		</view>
	</view>

</template>

<script>
	export default {
		data() {
			return {
				webSelf: this,
				count:1,
				num:1,
				proListDate:[
					{},{}
				]
			}
		},

		onLoad(options) {
			uni.setStorageSync('canClick', true);
		},

		onShow() {
			const self = this;
			document.title = ''
		},

		methods: {
			change(num){
				const self = this;
				if(num!=self.num){
					self.num = num
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
	@import "../../assets/style/car.css";
	page{background: #f5f5f5;padding-bottom: 140rpx;}

</style>
