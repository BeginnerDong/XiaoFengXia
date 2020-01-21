<template>
	<view>
		
		<view class="detailxqBan">
			<image :src="mainData.bannerImg&&mainData.bannerImg[0]?mainData.bannerImg[0].url:''" mode=""></image>
		</view>
		
		<view class="detail_one">
			<view class="flex">
				<view class="price font14">{{mainData.price}}</view>
				<view class="" style="font-size: 24rpx;padding: 0 10rpx;border-radius: 20rpx; line-height: 36rpx;background: #eee; color: #666; margin-left: 6rpx;">积分可抵用</view>
			</view>
			<view class="flexRowBetween color2 font13 number">
				<view class="">库存：{{mainData.stock?mainData.stock:''}}</view>
				<view class="">销量：{{mainData.sale_count?mainData.sale_count:''}}</view>
			</view>
			<view class="tit font14">{{mainData.title}}</view>
		</view>
		<view class="f5H10"></view>
		<!-- <view class="detal_guige pdlr4 flexRowBetween"  @click="showSel">
			<view class="flexRowAround ll">
				<view>规格</view>
				<view class="yyDate-lis flex">
					<view>10斤装</view>
					<view>10斤装</view>
					<view>10斤装</view>
				</view>
			</view>
			<view class="rr">
				<image class="arrowR" src="../../static/images/home-icon3.png" mode=""></image>
			</view>
		</view> -->
		<view class="f5H10"></view>
		<view class="infor-title">
			<view class="tt">详情介绍</view>
		</view>
		<view class="detailTexCont" style="padding: 20rpx 0;">
			<view class="center">
				<view class="content ql-editor"  style="padding: 12px 0;line-height: 0;"  v-html="mainData.content">
				</view>
			</view>
		</view>
		
		<!-- 底部菜单按钮 -->
		<view class="xqbotomBar">
			<view class="left flexRowBetween">
				<view class="ite" @click="prev">
					<image src="../../static/images/details=icon1.png" mode=""></image>
					<view>返回</view>
				</view>
			</view>
			<view class="payBtn flexRowBetween">
				<view class="item hei" style="width: 100%;" 
				@click="goBuy">
					<view class="price">{{mainData.score}}</view>
					<view class="name">结算</view>
				</view>
			</view>
		</view>
		
		<!-- 选择规格弹框 -->
		<view class="showSel" v-if="is_show" >
			<view class="showSelCont fix">
				<view class="colseBtn" @click="showSel">×</view>
				<view class="ind_proList">
					<view class="item flexRowBetween" style="padding:0rpx 0 20rpx 0;border-bottom: 0;">
						<view class="ll">
							<image src="../../static/images/home-img7.png" mode=""></image>
						</view>
						<view class="rr">
							<view class="avoidOverflow2 title">40枚正宗土鸡蛋农家散养新鲜农村野外放养笨鸡蛋草鸡蛋柴鸡蛋</view>
							<view class="money">
								<view class="flex left">
									<view class="price">26.8</view>
								</view>
							</view>
							
						</view>
					</view>
				</view>
				<view class="yy-title" style="padding: 10px 0;">规格</view>
				<view class="yyDate-lis flexRowBetween" style="flex-wrap: wrap; justify-content: flex-start;">
					<view v-for="(item,index) in seltSpecsList" :key="index" :class="[selCurrent == index? 'on':'']" @click="seltSpecs(index)">{{item}}</view>
				</view>
				<view class="flexRowBetween" style="padding: 30rpx 0; border-top: 2rpx solid #e7e7e7; border-bottom: 2rpx solid #e7e7e7; margin-top: 10rpx;">
					<view class="yy-title">购买数量</view>
					<view class="numBox" style="position: relative; right: auto; bottom: auto;">
						<view>+</view>
						<view class="num">1</view>
						<view>-</view>
					</view>
				</view>
				<view class="submitbtn" style="margin-top: 60rpx;">
					<button class="hei" style="margin-bottom: 0;" type="submit" 
					@click="Router.navigateTo({route:{path:'/pages/confirmOrder/confirmOrder'}})" >确定</button>
				</view>
			</view>
		</view>
		
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				Router: this.$Router,
				mainData: {}
			}
		},

		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
		},

		methods: {

			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.searchItem = {
					id: self.id
				}
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data[0];

						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			goBuy() {
				const self = this;
				if (JSON.stringify(self.mainData) == '{}') {
					api.showToast('商品错误', 'none', 1000);
					return;
				};
				var orderList = {
					product: [{
						id: self.mainData.id,
						count: 1,
						product: self.mainData
					}],
					type:self.mainData.type,
				};
				uni.setStorageSync('payPro', orderList);
				self.$Router.navigateTo({
					route: {
						path: '/pages/confirmOrderTwo/confirmOrderTwo'
					}
				})
			},
		}
	}
</script>

<style>
	@import "../../assets/style/page.css";
	@import "../../assets/style/car.css";
	page{padding-bottom: 140rpx;}
	.payBtn .hei .price::before{content: "积分：";}
	
	.showSel {width: 100%;height: 100%;background: rgba(0, 0, 0, 0.5);position: fixed;top: 0;right: 0;bottom: 0;left: 0;z-index: 10000;}
	.showSelCont {padding:40rpx 4%;background: #fff;}
	.showSelCont.fix{position: fixed;bottom: 0;left: 0;box-sizing: border-box;width: 100%;border-radius: 20rpx 20rpx 0 0;}
	.yy-title {font-size: 28rpx;}
	.yyDate-lis {max-height: 270rpx; overflow-y: auto;}
	.yyDate-lis view {width: 130rpx;height: 56rpx;font-size: 26rpx;color: #666;text-align: center;line-height: 56rpx;background: #f5f5f5;margin: 0 20rpx 30rpx 20rpx;}
	.yyDate-lis view.on{background: #F3D012; color: #333;}
	.showSel .numBox view {background: #f5f5f5;}
	.detal_guige{padding: 20rpx 4%;}
	.detal_guige .ll{width: 90%; display: flex; justify-content: flex-start;align-items: center;}
	.detal_guige .rr{width: 10%;display: flex;justify-content: flex-end;}
	.detal_guige .yyDate-lis view{margin:0 0 0 20rpx;}
</style>
