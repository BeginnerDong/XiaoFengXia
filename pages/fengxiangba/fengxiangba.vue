<template>
	<view>
		<view class="tooling_indNav">
			<view class="list">
				<view class="item" :class="index==1?'on':''" @click="changeone('1')">首页</view>
				<view class="item" :class="index==2?'on':''" @click="changeone('2')">活动</view>
			</view>
		</view>
		
		<view class="" v-if="index==1">
			<view class="fabubtn" @click="Router.navigateTo({route:{path:'/pages/fengxiangba_fabu/fengxiangba_fabu'}})">
				<view class="icon">
					<image src="../../static/images/beestoenjoy-icon7.png" mode=""></image>
				</view>
				<view class="tit">发布帖子</view>
			</view>
			<view class="interct_idexLis">
				<view class="child"  @click="Router.navigateTo({route:{path:'/pages/fengxiangba_pl/fengxiangba_pl'}})">
					<view class="flexRowBetween">
						<view class="flex">
							<view class="photo"><image src="../../static/images/beestoenjoy-img1.png" mode=""></image></view>
							<view class="font13 name avoidOverflow">快乐的猫</view>
						</view>
						<view class="gzBtn">关注ta</view>
					</view>
					<view class="title font14">标题标题标题标题标题标题标题标题</view>
					<view class="text font13">互动内容互动内容互动内容互动内容互动内容互动内容互动内容互动内容</view>
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
						<view class="lis">
							<image src="../../static/images/beestoenjoy-icon1.png" mode=""></image>
							<view>分享</view>
						</view>
					</view>
				</view>
				
				<view class="child"  @click="Router.navigateTo({route:{path:'/pages/fengxiangba_pl/fengxiangba_pl'}})">
					<view class="flexRowBetween">
						<view class="flex ">
							<view class="photo"><image src="../../static/images/beestoenjoy-img1.png" mode=""></image></view>
							<view class="font13 name avoidOverflow">快乐的猫</view>
						</view>
						
						<view class="gzBtn">关注ta</view>
					</view>
					<view class="title font14">标题标题标题标题标题标题标题标题</view>
					<view class="text font13">互动内容互动内容互动内容互动内容互动内容互动内容互动内容互动内容</view>
					<view class="imgbox">
						<view class="lisTwo">
							<image src="../../static/images/home-img7.png" mode=""></image>
						</view>
						<view class="lisTwo">
							<image src="../../static/images/beestoenjoy.png" mode=""></image>
						</view>
					</view>
					<view class="pltex_label">
						<view class="lis">
							<image src="../../static/images/beestoenjoy-icon4.png" mode=""></image>
							<view>1233</view>
						</view>
						<view class="lis">
							<image src="../../static/images/beestoenjoy-icon5.png" mode=""></image>
							<view>1236</view>
						</view>
						<view class="lis">
							<image src="../../static/images/beestoenjoy-icon6.png" mode=""></image>
							<view>12</view>
						</view>
						<view class="lis">
							<image src="../../static/images/beestoenjoy-icon1.png" mode=""></image>
							<view>分享</view>
						</view>
					</view>
				</view>
				
				<view class="child"  @click="Router.navigateTo({route:{path:'/pages/fengxiangba_pl/fengxiangba_pl'}})">
					<view class="flexRowBetween">
						<view class="flex ">
							<view class="photo"><image src="../../static/images/beestoenjoy-img1.png" mode=""></image></view>
							<view class="font13 name avoidOverflow">快乐的猫</view>
						</view>
						
						<view class="gzBtn">关注ta</view>
					</view>
					<view class="title font14">标题标题标题标题标题标题标题标题</view>
					<view class="text font13">互动内容互动内容互动内容互动内容互动内容互动内容互动内容互动内容</view>
					<view class="imgbox">
						<view class="lisOne">
							<image src="../../static/images/beestoenjoy.png" mode=""></image>
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
						<view class="lis">
							<image src="../../static/images/beestoenjoy-icon1.png" mode=""></image>
							<view>分享</view>
						</view>
					</view>
				</view>
				
			</view>
		</view>
		
		<view class="fx_avtive pdlr4"  v-if="index==2">
			<view class="item" style="background: #2e2e97;" @click="Router.navigateTo({route:{path:'/pages/activityDetail/activityDetail'}})">
				<view class="flexRowBetween">
					<view class="flex one">
						<view class="samllIcon"><image src="../../static/images/toobtain-icon2.png" mode=""></image></view>
						<view class="font10">小蜂侠优惠活动社群</view>
					</view>
					<view class="rIcon"><image src="../../static/images/beestoenjoy-icon8.png" mode=""></image></view>
				</view>
				<view class="infor flexRowBetween">
					<view class="bigLogo">
						<image src="../../static/images/beestoenjoy-img5.png" alt="" />
					</view>
					<view class="title">吴晓波深圳城市圈</view>
				</view>
			</view>
			<view class="item" style="background: #8632b2;"  @click="Router.navigateTo({route:{path:'/pages/activityDetail/activityDetail'}})">
				<view class="flexRowBetween">
					<view class="flex one">
						<view class="samllIcon"><image src="../../static/images/toobtain-icon2.png" mode=""></image></view>
						<view class="font10">小蜂侠优惠活动社群</view>
					</view>
					<view class="rIcon"><image src="../../static/images/beestoenjoy-icon8.png" mode=""></image></view>
				</view>
				<view class="infor flexRowBetween">
					<view class="bigLogo">
						<image src="../../static/images/beestoenjoy-img5.png" alt="" />
					</view>
					<view class="title">WorkFace深圳</view>
				</view>
			</view>
			<view class="item" style="background: #23a156;"  @click="Router.navigateTo({route:{path:'/pages/activityDetail/activityDetail'}})">
				<view class="flexRowBetween">
					<view class="flex one">
						<view class="samllIcon"><image src="../../static/images/toobtain-icon2.png" mode=""></image></view>
						<view class="font10">小蜂侠优惠活动社群</view>
					</view>
					<view class="rIcon"><image src="../../static/images/beestoenjoy-icon8.png" mode=""></image></view>
				</view>
				<view class="infor flexRowBetween">
					<view class="bigLogo">
						<image src="../../static/images/beestoenjoy-img5.png" alt="" />
					</view>
					<view class="title">WorkFace广州</view>
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
				wx_info:{},
				index:1
			}
		},
		
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			changeone(index){
				const self=this;
				if(index!=self.index){
					self.index = index
				}
			},
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
	@import "../../assets/style/page.css";

	page {
		padding-bottom: 60rpx;
	}
	
</style>
