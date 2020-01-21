<template>
	<view>
		<view class="detailxqBan" style="height: 400rpx;">
			<image :src="mainData.bannerImg&&mainData.bannerImg[0]?mainData.bannerImg[0].url:''" mode=""></image>
		</view>
		
		<view class="fabubtn" @click="Router.navigateTo({route:{path:'/pages/activityJoin/activityJoin?id='+mainData.id}})">
			<view class="icon">
				<image src="../../static/images/activity-icon1.png" mode=""></image>
			</view>
		</view>
		
		<view class="xqInfor" style="padding-top: 30rpx;">
			<view class="title" style="padding-bottom: 30rpx;">
				<view class="center font15" >{{mainData.title}}</view>
			</view>
			<view class="content ql-editor" v-html="mainData.content">
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
				postData.saveFunction = [{
					FuncName: 'viewArticle',
					searchItem:{
						id:self.id
					}
				}];
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
				self.$apis.articleGet(postData, callback);
			},
		}
	}
</script>

<style>
	@import "../../assets/style/page.css";
	@import "../../assets/style/quill.css";
</style>
