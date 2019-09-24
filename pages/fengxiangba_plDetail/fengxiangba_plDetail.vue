<template>
	<view>
		
		<view class="fxPlunCont">
			<view class="child" style="border-bottom: 0;">
				<view class="photo"><image src="../../static/images/about-img1.png" mode=""></image></view>
				<view class="infor">
					<view class="name font13">快乐的猫</view>
					<view class="time">09-21</view>
					<view class="title">重新拔插下内存试试，昨天我的电脑也开不了机，试了一下长按10秒钟试一试</view>
					<view class="pl">
						<view class="lis">
							<text class="plNam">张三：</text>
							<text>这个方法我试过，是有效的。这个方法我试过，是有效的</text>
						</view>
						<view class="lis">
							<text class="plNam">李回鹘：</text>
							<text>多试试几遍，或者关机过会在开机试试，不行的话再咨询客服</text>
						</view>
						<view class="lis">
							<text class="plNam">张三：</text>
							<text>这个方法我试过，是有效的。这个方法我试过，是有效的</text>
						</view>
						<view class="lis">
							<text class="plNam">李回鹘：</text>
							<text>多试试几遍，或者关机过会在开机试试，不行的话再咨询客服</text>
						</view>
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
				</view>
			</view>
		</view>
	
		<view class="interDtFixd flexRowBetween">
			<input type="text" value="" placeholder="说点什么" />
			<view class="sendBtn">发送</view>
		</view>
	</view>
</template>

<script>
	
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				wx_info:{}
			}
		},
		
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
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
	@import "../../assets/style/page.css";

	page {padding-bottom: 140rpx;}
	
	
	
	
</style>
