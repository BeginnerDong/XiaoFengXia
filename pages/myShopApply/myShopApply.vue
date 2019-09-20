<template>
	<view class="pub_editInfor">
		<view class="item">
			<view class="name">姓名</view>
			<view class="rr">
				<input type="text" placeholder="请输入姓名"/>
			</view>
		</view>
		<view class="item">
			<view class="name">电话</view>
			<view class="rr">
				<input type="number" maxlength="11" placeholder="请输入电话号码"/>
			</view>
		</view>
		<view class="item">
			<view class="name">门店名称</view>
			<view class="rr">
				<input type="text" placeholder="请输入门店名称"/>
			</view>
		</view>
		<view class="item">
			<view class="name">小区名称</view>
			<view class="rr">
				<input type="text" placeholder="请输入小区名称"/>
			</view>
		</view>
		<view class="item">
			<view class="name">小区地址</view>
			<view class="rr">
				<input type="text" placeholder="请输入小区地址"/>
			</view>
		</view>
		<view class="item">
			<view class="name">负责人</view>
			<view class="rr">
				<input type="text" placeholder="请输入负责人(部门维护人员)姓名"/>
			</view>
		</view>
		
		
		<view class="submitbtn" style="margin-top: 200rpx;">
			<button class="hei" type="submit" @click="webSelf.$Router.navigateTo({route:{path:'/pages/myShopApply_pay/myShopApply_pay'}})">确定</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				webSelf: this,
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

 
