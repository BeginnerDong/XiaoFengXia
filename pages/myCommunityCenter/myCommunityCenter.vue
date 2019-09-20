<template>
	<view>
		<view class="orderNav orderNavThree">
			<view class="tt" :class="current==1?'on':''" @click="change('1')">待审核</view>
			<view class="tt" :class="current==2?'on':''" @click="change('2')">已发布</view>
			<view class="tt" :class="current==3?'on':''" @click="change('3')">关注</view>
			<view class="tt" :class="current==4?'on':''" @click="change('4')">收藏</view>
			<view class="tt" :class="current==5?'on':''" @click="change('5')">评论</view>
		</view>
		
		<view class="myComunityCt">
			<view class="interct_idexLis" v-if="current==1">
				<view class="child" v-for="(item,index) in shimgData" :key="index">
					<view class="flexRowBetween font12 shTime">
						<view class="">审核日期：2019.09.20 10:38:30</view>
						<view class="red">未审核</view>
					</view>
					<view class="title font14">标题标题标题标题标题标题标题标题</view>
					<view class="text font13">各位大佬帮我分析一下电脑主机被家里弄的返潮，开不了机是怎么回事?</view>
					<view class="imgbox">
						<view class="lisThree">
							<image src="../../static/images/home-img7.png" mode=""></image>
						</view>
						<view class="lisThree">
							<image src="../../static/images/beestoenjoy.png" mode=""></image>
						</view>
						<view class="lisThree">
							<image src="../../static/images/order-img1.png" mode=""></image>
						</view>
					</view>
				</view>
			</view>
			<view class="interct_idexLis" v-if="current==2">
				<view class="child" v-for="(item,index) in shimgData" :key="index" @click="webSelf.$Router.navigateTo({route:{path:'/pages/fengxiangba_pl/fengxiangba_pl'}})">
					<view class="title font14">标题标题标题标题标题标题标题标题</view>
					<view class="text font13">各位大佬帮我分析一下电脑主机被家里弄的返潮，开不了机是怎么回事?</view>
					<view class="imgbox">
						<view class="lisThree">
							<image src="../../static/images/home-img7.png" mode=""></image>
						</view>
						<view class="lisThree">
							<image src="../../static/images/beestoenjoy.png" mode=""></image>
						</view>
						<view class="lisThree">
							<image src="../../static/images/order-img1.png" mode=""></image>
						</view>
					</view>
					<view class="pltex_label">
						<view class="lis">
							<image src="../../static/images/beestoenjoy-icon4.png" mode=""></image>
							<view>1233</view>
						</view>
						<view class="lis">
							<image src="../../static/images/beestoenjoy-icon3.png" mode=""></image>
							<view>1236</view>
						</view>
						<view class="lis">
							<image src="../../static/images/beestoenjoy-icon2.png" mode=""></image>
							<view>12</view>
						</view>
					</view>
				</view>
			</view>
			
			<view class="myRowBetween"  v-if="current==3">
				<view class="item flexRowBetween radius10" v-for="(item,indx) in followDate" :key="index">
					<view class="left">
						<view class="photo">
							<image src="../../static/images/focusOn-img1.png" mode=""></image>
						</view>
						<view>流浪歌哥</view>
					</view>
					<view class="right"><view class="gzOkBtn">已关注</view></view>
				</view>
			</view>
			<view class="interct_idexLis" v-if="current==4">
				<view class="child" v-for="(item,index) in shimgData" :key="index" @click="webSelf.$Router.navigateTo({route:{path:'/pages/fengxiangba_pl/fengxiangba_pl'}})">
					<view class="title font14">标题标题标题标题标题标题标题标题</view>
					<view class="text font13">各位大佬帮我分析一下电脑主机被家里弄的返潮，开不了机是怎么回事?</view>
					<view class="imgbox">
						<view class="lisThree">
							<image src="../../static/images/home-img7.png" mode=""></image>
						</view>
						<view class="lisThree">
							<image src="../../static/images/beestoenjoy.png" mode=""></image>
						</view>
						<view class="lisThree">
							<image src="../../static/images/order-img1.png" mode=""></image>
						</view>
					</view>
					<view class="pltex_label">
						<view class="lis">
							<image src="../../static/images/beestoenjoy-icon4.png" mode=""></image>
							<view>1233</view>
						</view>
						<view class="lis">
							<image src="../../static/images/beestoenjoy-icon3.png" mode=""></image>
							<view>1236</view>
						</view>
						<view class="lis">
							<image src="../../static/images/beestoenjoy-icon6.png" mode=""></image>
							<view>12</view>
						</view>
					</view>
				</view>
			</view>
			<view class="interct_idexLis" v-if="current==5">
				<view class="child" v-for="(item,index) in shimgData" :key="index" @click="webSelf.$Router.navigateTo({route:{path:'/pages/fengxiangba_pl/fengxiangba_pl'}})">
					<view class="title font14">标题标题标题标题标题标题标题标题</view>
					<view class="text font13">各位大佬帮我分析一下电脑主机被家里弄的返潮，开不了机是怎么回事?</view>
					<view class="imgbox">
						<view class="lisThree">
							<image src="../../static/images/home-img7.png" mode=""></image>
						</view>
						<view class="lisThree">
							<image src="../../static/images/beestoenjoy.png" mode=""></image>
						</view>
						<view class="lisThree">
							<image src="../../static/images/order-img1.png" mode=""></image>
						</view>
					</view>
					<view class="pltex_label">
						<view class="lis">
							<image src="../../static/images/beestoenjoy-icon4.png" mode=""></image>
							<view>1233</view>
						</view>
						<view class="lis">
							<image src="../../static/images/beestoenjoy-icon3.png" mode=""></image>
							<view>1236</view>
						</view>
						<view class="lis">
							<image src="../../static/images/beestoenjoy-icon2.png" mode=""></image>
							<view>12</view>
						</view>
					</view>
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
				score: '',
				wx_info: {},
				current:1,
				shimgData:[
					{},{}
				],
				followDate:[
					{},{},{},{},{}
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

	.orderNavThree .tt{ width: 20%;}
	.gzOkBtn{font-size: 24rpx;width: 120rpx; text-align: center; line-height: 44rpx; background: #e1e1e1;border-radius: 30rpx;}
</style>
