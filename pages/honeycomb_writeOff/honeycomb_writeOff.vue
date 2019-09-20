<template>
	<view>
		<view class="tooling_indNav">
			<view class="list">
				<view class="item" :class="index==1?'on':''" @click="changeone('1')">未核销</view>
				<view class="item" :class="index==2?'on':''" @click="changeone('2')">已核销</view>
			</view>
		</view>
		<view v-if="index==1">
			<view class="prolisbox pdlr4">
				<view class="prolis boxShaow" v-for="(item,index) in orderDate" :key="index">
					<view class="datt">
						<view class="left">
							<view class="color2" style="margin-bottom: 10rpx;">交易时间：2019-09-20</view>
						</view>
						<view class="state">未核销</view>
					</view>
					<view class="ind_proList">
						<view class="item flexRowBetween">
							<view class="ll">
								<image src="../../static/images/order-img1.png" mode=""></image>
							</view>
							<view class="rr">
								<view class="avoidOverflow2 title">40枚正宗土鸡蛋农家散养新鲜农村野外放养笨鸡蛋草鸡蛋柴鸡蛋</view>
								<view class="lable">
									<view class="tt">3斤装</view>
								</view>
								<view class="money flexRowBetween">
									<view class="flex left">
										<view class="price">59.00</view>
									</view>
								</view>
							</view>
						</view>
					</view>
					<view class="myComunityCt" style="margin: 0;">
						<view class="myRowBetween">
							<view class="item flexRowBetween">
								<view class="left flex" style="width: 60%;">
									<view class="photo">
										<image src="../../static/images/about-img1.png" mode=""></image>
									</view>
									<view class="font13">
										<view class="font13 avoidOverflow">昵称昵称昵称</view>
										<view class="time">15232302210</view>
									</view>
								</view>
								<view class="right red font12" style="width: 40%;">
									<view class="font14">可得佣金600</view>
								</view>
							</view>
						</view>
					</view>
					<view class="bBtn">
						<view class="btn">取货提示</view>
					</view>
				</view>
			</view>
		</view>
		<view v-if="index==2">
			<view class="prolisbox pdlr4">
				<view class="prolis boxShaow" v-for="(item,index) in orderDate" :key="index">
					<view class="datt">
						<view class="left">
							<view class="color2" style="margin-bottom: 10rpx;">交易时间：2019-09-20</view>
						</view>
						<view class="state">已核销</view>
					</view>
					<view class="ind_proList">
						<view class="item flexRowBetween">
							<view class="ll">
								<image src="../../static/images/order-img1.png" mode=""></image>
							</view>
							<view class="rr">
								<view class="avoidOverflow2 title">40枚正宗土鸡蛋农家散养新鲜农村野外放养笨鸡蛋草鸡蛋柴鸡蛋</view>
								<view class="lable">
									<view class="tt">3斤装</view>
								</view>
								<view class="money flexRowBetween">
									<view class="flex left">
										<view class="price">59.00</view>
									</view>
								</view>
							</view>
						</view>
					</view>
					<view class="myComunityCt" style="margin: 0;">
						<view class="myRowBetween">
							<view class="item flexRowBetween">
								<view class="left flex" style="width: 60%;">
									<view class="photo">
										<image src="../../static/images/about-img1.png" mode=""></image>
									</view>
									<view class="font13">
										<view class="font13 avoidOverflow">昵称昵称昵称</view>
										<view class="time">15232302210</view>
									</view>
								</view>
								<view class="right red font12" style="width: 40%;">
									<view class="font14">可得佣金600</view>
								</view>
							</view>
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
				index:1,
				orderDate:[
					{},{}
				]
			}
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
				self.$apis.userGet(postData, callback);
			}
		}
	}
</script>

<style>
	@import "../../assets/style/page.css";
	@import "../../assets/style/user.css";
	page{ background: #f5f5f5;padding-bottom: 50rpx;}

	.tooling_indNav .list .item{width: 50%;}
	.prolisbox .prolis{ margin: 0 auto 30rpx auto;}
	.myComunityCt .myRowBetween .item{padding: 0;margin: 30rpx 0 0 0;}
	
	.prolisbox .prolis .ind_proList .item{padding: 0;border-bottom: 0;}
	.prolisbox .prolis .ind_proList .item .ll{width: 192rpx; height: 192rpx;}
	.prolisbox .prolis .ind_proList .item .rr{width: 66%; height: 192rpx;}
</style>
