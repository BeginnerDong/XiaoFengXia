<template>
	<view>
		<view class="tooling_indNav">
			<view class="list">
				<view class="item" :class="index==1?'on':''" @click="changeone('1')">自提订单</view>
				<view class="item" :class="index==2?'on':''" @click="changeone('2')">快递订单</view>
			</view>
		</view>
		<view v-if="index==1">
			<view class="orderNav orderNavThree">
				<view class="tt" :class="current==1?'on':''" @click="change('1')">全部</view>
				<view class="tt" :class="current==2?'on':''" @click="change('2')">待核销</view>
				<view class="tt" :class="current==3?'on':''" @click="change('3')">已完成</view>
			</view>
			<view class="prolisbox pdlr4">
				<view class="prolis boxShaow">
					<view class="datt">
						<view class="left">
							<view class="color2" style="margin-bottom: 10rpx;">订单编号：123356885555</view>
						</view>
						<view class="state">待核销</view>
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
					<view class="bBtn">
						<view class="btn"  @click="Router.navigateTo({route:{path:'/pages/myOrder_hexiao/myOrder_hexiao'}})">待核销</view>
					</view>
				</view>
				<view class="prolis boxShaow">
					<view class="datt">
						<view class="left">
							<view class="color2" style="margin-bottom: 10rpx;">订单编号：123356885555</view>
						</view>
						<view class="state">已完成</view>
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
				</view>
				
			</view>
		</view>
		<view v-if="index==2">
			<view class="orderNav">
				<view class="tt" :class="current==1?'on':''" @click="change('1')">全部</view>
				<view class="tt" :class="current==2?'on':''" @click="change('2')">待发货</view>
				<view class="tt" :class="current==3?'on':''" @click="change('3')">已发货</view>
				<view class="tt" :class="current==4?'on':''" @click="change('4')">已完成</view>
			</view>
			<view class="prolisbox pdlr4">
				<view class="prolis boxShaow">
					<view class="datt">
						<view class="left">
							<view class="color2" style="margin-bottom: 10rpx;">订单编号：123356885555</view>
						</view>
						<view class="state">待发货</view>
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
					
				</view>
				<view class="prolis boxShaow">
					<view class="datt">
						<view class="left">
							<view class="color2" style="margin-bottom: 10rpx;">订单编号：123356885555</view>
						</view>
						<view class="state">已发货</view>
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
						<view class="bBtn">
							<view class="btn"  @click="Router.navigateTo({route:{path:'/pages/myOrder_hexiao/myOrder_hexiao'}})">确认收货</view>
						</view>
					</view>
				</view>
				<view class="prolis boxShaow">
					<view class="datt">
						<view class="left">
							<view class="color2" style="margin-bottom: 10rpx;">订单编号：123356885555</view>
						</view>
						<view class="state">已完成</view>
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
				score: '',
				wx_info: {},
				current:1,
				index:1
			}
		},

		methods: {
			change(current) {
				const self = this;
				if(current!=self.current){
					self.current = current
				}
			},
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
	page{ background: #f5f5f5;padding-bottom: 80rpx;}

	.tooling_indNav .list .item{width: 50%;}
	.prolis3 .cont{padding: 20rpx 0;box-sizing: border-box;}
	.prolis3 .tex{margin-top: 20rpx;}
	.orderNavThree .tt{ width: 33.3%;}
	
	.prolisbox .prolis .ind_proList .item{padding: 0;border-bottom: 0;}
	.prolisbox .prolis .ind_proList .item .ll{width: 192rpx; height: 192rpx;}
	.prolisbox .prolis .ind_proList .item .rr{width: 66%; height: 192rpx;}
</style>
