<template>
	<view>

		<view class="userHead">
			<view class="left flex">
				<view class="photo">
					<open-data type="userAvatarUrl"></open-data>
				</view>
				<view class="infor" >
					<view class="font14"><open-data type="userNickName"></open-data></view>
				</view>
			</view>
		</view>
		
		<view class="f5H10"></view>
		<view class="XlineNav">
			<view class="info" @click="scan">
				<view class="ilblock listimg">
					<image src="../../static/images/merchants-icon1.png"></image>
				</view>
				<view class="ilblock">扫一扫</view>
				<image class="arrowR" src="../../static/images/home-icon3.png" ></image>
			</view>
			<view class="info" @click="Router.navigateTo({route:{path:'/pages/honeycomb_shopMsg/honeycomb_shopMsg'}})">
				<view class="ilblock listimg">
					<image src="../../static/images/merchants-icon2.png"></image>
				</view>
				<view class="ilblock">门店管理</view>
				<image class="arrowR" src="../../static/images/home-icon3.png" ></image>
			</view>
			<view class="info" @click="Router.navigateTo({route:{path:'/pages/honeycomb_writeOff/honeycomb_writeOff'}})">
				<view class="ilblock listimg">
					<image src="../../static/images/merchants-icon4.png"></image>
				</view>
				<view class="ilblock">核销订单</view>
				<image class="arrowR" src="../../static/images/home-icon3.png" ></image>
			</view>
			<view class="info" @click="Router.navigateTo({route:{path:'/pages/myYongJin/myYongJin?type=merchant'}})">
				<view class="ilblock listimg">
					<image src="../../static/images/merchants-icon3.png"></image>
				</view>
				<view class="ilblock">我的佣金</view>
				<image class="arrowR" src="../../static/images/home-icon3.png" ></image>
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
				wx_info:{}
			}
		},
		
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			scan(){
				const self = this;
				uni.scanCode({
				    success: function (res) {
				        console.log('条码类型：' + res.scanType);
				        console.log('条码内容：' + res.result);
						self.id = res.result;
						self.getOrderData(res.result)
				    }
				});
			},
			
			getOrderData(id) {
				const self = this;
				const postData = {};
				postData.searchItem = {
					id:id,
					shop_no:uni.getStorageSync('merchantInfo').user_no,
					pay_status:1,
					order_step:['in',[0,5]],
					check_status:0,
					user_type:0
				};
				postData.tokenFuncName = 'getMerchantToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.orderData = res.info.data[0]
						self.orderUpdate(id)
					} else {
						self.$Utils.showToast('订单异常', 'none');
					};
				};
				self.$apis.orderGet(postData, callback);
			},
			
			orderUpdate(id) {
				const self = this;
				const postData = {};
				postData.searchItem = {
					id:id,
					user_type:0
				};
				postData.data = {
					check_status:1
				};
				/* postData.saveAfter = [
					{
						tableName: 'FlowLog',
						FuncName: 'add',
						data: {
							type: 2,
							relation_id:1,
							relation_table:'order',
							trade_info:'门店返佣',
							user_no:uni.getStorageSync('merchantInfo').user_no,
							thirdapp_id:2,
							count:self.orderData.products[0].snap_product.product.shop_reward*self.orderData.products[0].count
						}
					}
				];	 */
				postData.tokenFuncName ='getMerchantToken';
				const callback = (res) => {
					if(res.solely_code==100000){
						self.$Utils.showToast('已核销', 'none');
						/* setTimeout(function() {
							self.$Router.redirectTo({route:{path:'/pages/myOrder/myOrder'}})
						}, 1000); */
					}
				};
				self.$apis.orderUpdate(postData, callback);
			},
			
			
			
		},
	};
</script>

<style>
	@import "../../assets/style/user.css";

</style>
