<template>
	<view class="">
		<view class="myRowBetween" >
			<view class="item flexRowBetween" v-for="(item,index) in rewardData" :key="index">
				<view class="left">
					<view>获得200积分</view>
					<view class="time">2019-09-19</view>
				</view>
				<view class="right pubColor">+200</view>
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
				score:'',
				wx_info:{},
				rewardData:[
					{},{},{},{},{}
				]
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


 
