<template>
	<view>
		
		<view class="xqInfor" style="padding-top: 30rpx;">
			<view class="title">
				<view class="center font15" style="padding: 30rpx 0;">标题标题标题标题标题标题</view>
				<view class="font13 color3" style="padding-bottom: 30rpx; text-align: right;" >2019-9-10 17:43</view>
			</view>
			
			<view class="cont">
				<view class="title">一、标题标题标题标题标题标题标题</view>
				<view>1、内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容</view>
				<view>2、内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容</view>
				<view>3、内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容</view>
				<view>4、内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容</view>
				<view>
					<image src="../../static/images/activity-img2.png" mode="widthFix"></image>
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
				score:'',
				wx_info:{},
				is_show:false,
				imgUrls: [
					"../../static/images/home-banenr.png",
					"../../static/images/details-img1.png"
				],
				designPjLis:[
					{},{},{},{}
				]
			}
		},

		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},

		methods: {
			getMainData() {
				const self = this;
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
			
			}
		}
	}
</script>

<style>
	@import "../../assets/style/page.css";
	@import "../../assets/style/quill.css";
</style>
