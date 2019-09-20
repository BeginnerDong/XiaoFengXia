<template>
	<view class="pub_editInfor">
		<view class="item">
			<view class="name">地址</view>
			<view class="rr">
				<input type="text" value="陕西省西安市雁塔区高新大都荟" placeholder="请输入姓名"/>
			</view>
		</view>
		<view class="item">
			<view class="name">门店名称</view>
			<view class="rr">
				<input type="text" value="胖哥两" placeholder="请输入门店名称"/>
			</view>
		</view>
		<view class="item">
			<view class="name">联系电话</view>
			<view class="rr">
				<input type="number" maxlength="11" placeholder="15623235656"/>
			</view>
		</view>
		
		
		
		<view class="submitbtn" style="margin-top: 200rpx;">
			<button class="hei" type="submit">确定</button>
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

 
