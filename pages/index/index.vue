<template>
	<view>
		<form @submit="formIdAdd" report-submit="true">


			<view class="index_topBj">
				<view class="adrsGps" @click="Router.navigateTo({route:{path:'/pages/goodsOpenDetailTwo/goodsOpenDetailTwo?city='+city}})">
					<view class="dd">{{city}}</view>
					<image class="icon" src="../../static/images/home-icon1.png" mode=""></image>
				</view>
				<view class="banner-box">
					<view class="banner radius10 oh">
						<swiper class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000"
						 indicator-active-color="#434343">
							<block v-for="(item,index) in swiperData" :key="index">
								<swiper-item class="swiper-item" :data-url="item.url"
								  @click="Router.navigateTo({route:{path:$event.currentTarget.dataset.url}})">
									<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" class="slide-image" />
								</swiper-item>
							</block>
						</swiper>
					</view>
				</view>

			</view>
			<view class="pdlr4 inde_fivePic">
				<view class="flexRowBetween">
					<view class="item" style="width: 340rpx; height: 240rpx;" @click="intoNew">
						<image src="../../static/images/home-img1.jpg" mode=""></image>
						<!-- <view class="textbox">
							<view class="tit">新人团</view>
							<view class="tex">点击进入</view>
							<view class="font12">&gt;&gt;&gt;</view>
						</view> -->
					</view>

					<view class="item" style="width: 330rpx; height: 240rpx;" @click="intoStore">
						<image src="../../static/images/home-img2.jpg" mode=""></image>
						<!-- <view class="textbox">
							<view class="tit">我的店铺</view>
							<view class="tex">点击进入</view>
							<view class="font12">&gt;&gt;&gt;</view>
						</view> -->
					</view>
				</view>
				<view class="flexRowBetween threebox">
					<view class="item" @click="Router.navigateTo({route:{path:labelData.url}})">
						<image src="../../static/images/home-img3.jpg" mode=""></image>
						<!-- <view class="textbox">
							<view class="tit">{{labelData.description}}</view>
							<view class="tex">吃出好生活</view>
						</view> -->
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/coupon/coupon'}})">
						<image src="../../static/images/home-img4.jpg" mode=""></image>
						<!-- <view class="textbox">
							<view class="tit">优惠券</view>
							<view class="tex">尽情畅买</view>
						</view> -->
					</view>
					<view class="item" @click="toTab">
						<image src="../../static/images/home-img5.jpg" mode=""></image>
						<!-- <view class="textbox">
							<view class="tit">活动中心</view>
							<view class="tex">玩遍天南海北</view>
						</view> -->
					</view>
				</view>

			</view>

			<view class="f5H10"></view>
			<view class="infor-title flexRowBetween">
				<view class="tt">常规团</view>
				<view class="more" @click="Router.navigateTo({route:{path:'/pages/routine/routine'}})">更多&gt;</view>
			</view>
			<view class="pdlr4">
				<view style="padding-top: 30rpx;width: 100%; height: 260rpx;" @click="Router.navigateTo({route:{path:labelTwoData.url}})">
					<image style="width: 100%; height: 100%; display: block;border-radius: 10rpx;" :src="labelTwoData.mainImg&&labelTwoData.mainImg[0]?labelTwoData.mainImg[0].url:''"></image>
				</view>

				<view class="ind_proList">
					<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index" 
					:data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/goodsJoinDetail/goodsJoinDetail?id='+$event.currentTarget.dataset.id}})">
						<view class="ll">
							<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
						</view>
						<view class="rr">
							<view class="avoidOverflow2 title">{{item.title?item.title:''}}</view>
							<view class="lable">
								<view class="tt">{{item.score?item.score:''}}积分</view>
								<view class="tt">{{item.standard?item.standard:''}}人团</view>
							</view>
							<view class="money flexRowBetween">
								<view class="flex left">
									<view class="price">{{item.price?item.price:''}}</view>
									<view class="yuan price">{{item.o_price?item.o_price:''}}</view>
								</view>
								<button form-type="submit" style="margin: 0;" class="addbtn">{{item.order&&item.order.length==0?'开团':'参团'}}</button>
							</view>

						</view>
					</view>
				</view>
			</view>
		</form>
	</view>

</template>

<script>
	export default {
		data() {
			return {
				Router: this.$Router,
				indProDate: [{}, {}, {}, {}],
				swiperData: [],
				mainData: [],
				labelData: {},
				city: '',
				labelTwoData: {},
				userInfoData: {}
			}
		},

		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			const callback = (res) => {
				self.$Utils.loadAll(['getSliderData', 'getLabelData', 'getLocation', 'getLabelTwoData'], self);
			};
			self.$Token.getProjectToken(callback, {
				refreshToken: true
			})
		},


		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},

		onPullDownRefresh() {
			console.log('refresh');
			const self = this;
			self.$Utils.loadAll(['getSliderData', 'getLabelData', 'getLocation', 'getLabelTwoData'], self);
			setTimeout(function() {
				uni.stopPullDownRefresh();
			}, 1000);
		},

		onShow() {
			const self = this;
			if (uni.getStorageSync('city')) {
				self.mainData = [];
				self.city = uni.getStorageSync('city');
				self.getCityData();
				uni.removeStorageSync('city')
			}
			self.getUserInfoData()
		},

		methods: {
			
			toTab(){
				const self = this;
				uni.setStorageSync('index',2);
				self.Router.switchTab({route:{path:'/pages/fengxiangba/fengxiangba'}})
			},

			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no: uni.getStorageSync('user_info').user_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0];
					}
				};
				self.$apis.userInfoGet(postData, callback);
			},

			formIdAdd(e) {
				const self = this;
				console.log(e)

				self.$apis.WxFormIdAdd(e.detail.formId, Date.parse(new Date()) / 1000 + 7 * 86400);
			},

			getLocation() {
				const self = this;
				const callback = (res) => {
					if (res) {
						console.log('res', res)
						if (res.authSetting) {
							console.log(232)
							return
						}
						self.city = res.address_component.city
						self.getCityData()
					};
				};
				self.$Utils.getLocation('reverseGeocoder', callback);

			},

			getCityData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					title: self.city
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.cityData = res.info.data[0];
						self.city_id = self.cityData && self.cityData.id ? self.cityData.id : 24;
						self.getMainData(true)
					} else {
						uni.showModal({
							title: '提示',
							content: '当前城市未开通，是否切换为默认城市西安',
							success: function(res) {
								if (res.confirm) {
									self.city_id = self.cityData && self.cityData.id ? self.cityData.id : 24;
									self.city = '西安市';
									self.getMainData(true)
								} else if (res.cancel) {
									console.log('用户点击取消');
								}
							}
						});
					}

				};
				self.$apis.labelGet(postData, callback);
			},

			getLabelData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					title: '首页推荐位'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelData = res.info.data[0]
					}
					console.log('self.swiperData', self.swiperData)
					self.$Utils.finishFunc('getLabelData');
				};
				self.$apis.labelGet(postData, callback);
			},

			getLabelTwoData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					title: '常规团推荐位'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelTwoData = res.info.data[0]
					}
					console.log('self.swiperData', self.swiperData)
					self.$Utils.finishFunc('getLabelTwoData');
				};
				self.$apis.labelGet(postData, callback);
			},

			intoStore() {
				const self = this;
				if (self.userInfoData.check_status == 0) {
					self.Router.navigateTo({
						route: {
							path: '/pages/myShopApply/myShopApply'
						}
					})
				} else if (self.userInfoData.check_status == 1) {
					self.$Utils.showToast('您的资料审核中', 'none');
				} else if (self.userInfoData.check_status == 2 && self.userInfoData.level >= 3) {
					self.Router.navigateTo({
						route: {
							path: '/pages/myShop/myShop'
						}
					})
				}
			},

			intoNew() {
				const self = this;
				if (self.userInfoData.level == 0) {
					self.Router.navigateTo({
						route: {
							path: '/pages/newPeople/newPeople'
						}
					})
				} else {
					self.$Utils.showToast('您已不是新人身份', 'none');
				}
			},

			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 5
					}
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id: 2,
					type: 1,
					level: ['>', 0],
					category_id: ['not in', [57]],
					area_id: self.city_id
				};
				postData.getAfter = {
					order: {
						token: uni.getStorageSync('user_token'),
						tableName: 'Order',
						middleKey: 'product_no',
						key: 'product_no',
						searchItem: {
							status: 1,
							order_step: 4,
							pay_status: 1
						},
						condition: '='
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getLocation');
				};
				self.$apis.productGet(postData, callback);
			},

			getSliderData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
				};
				postData.getBefore = {
					caseData: {
						tableName: 'Label',
						searchItem: {
							title: ['=', ['首页轮播图']],
						},
						middleKey: 'parentid',
						key: 'id',
						condition: 'in',
					},
				};
				postData.getAfter = {
					Article: {
						tableName: 'Article',
						searchItem: {
							status: 1,
						},
						middleKey: 'id',
						key: 'menu_id',
						condition: 'in',
						info: ['id']
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.swiperData.push.apply(self.swiperData, res.info.data)
					}
					console.log('self.swiperData', self.swiperData)
					self.$Utils.finishFunc('getSliderData');
				};
				self.$apis.labelGet(postData, callback);
			},
		},

	};
</script>

<style>
	@import "../../assets/style/page.css";

	page {
		padding-bottom: 60rpx;
	}

	.index_topBj {
		background: #fff url(../../static/images/home-bBj.png) no-repeat 0 0/100% auto;
		padding-top: 0
	}
</style>
