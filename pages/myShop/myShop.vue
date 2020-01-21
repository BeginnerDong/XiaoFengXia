<template>
	<view>
		<view class="index_topBj">
			<view class="banner-box">
				<view class="banner radius10 oh">
					<swiper class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000" indicator-active-color="#434343">
						<block v-for="(item,index) in labelData.mainImg" :key="index">
							<swiper-item class="swiper-item">
								<image :src="item.url" class="slide-image" />
							</swiper-item>
						</block>
					</swiper>
				</view>
			</view>
		</view>
		
		<view class="f5H10"></view>
		
		<view class="orderNav">
			<view class="tt" :class="current==1?'on':''" @click="change('1')">尝鲜区</view>
			<view class="tt" :class="current==2?'on':''" @click="change('2')">全部商品</view>
		</view>
		<view class="pdlr4" v-if="mainData.length>0">
			<view class="ind_proList" v-if="current==1">
				<view class="item flexRowBetween" v-for="(item,index) in mainData" :data-id="item.id"
				:key="index" @click="Router.navigateTo({route:{path:'/pages/myShopPayDetail/myShopPayDetail?id='+$event.currentTarget.dataset.id}})">
					<view class="ll">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
					</view>
					<view class="rr">
						<view class="avoidOverflow2 title">{{item.title}}</view>
						<view class="lable">
							<view class="tt">{{item.score}}积分</view>
						</view>
						<view class="money flexRowBetween">
							<view class="flex left">
								<view class="price">{{item.price}}</view>
								<view class="yuan price">{{item.o_price}}</view>
							</view>
							<view class="addbtn">购买</view>
						</view>
					</view>
				</view>
			</view>
			
			<view class="ind_proList" v-if="current==2">
				<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index"  :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/goodsJoinDetail/goodsJoinDetail?user_no='+user_no+'&id='+$event.currentTarget.dataset.id}})">
					<view class="ll">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
					</view>
					<view class="rr">
						<view class="avoidOverflow2 title">{{item.title}}</view>
						<view class="lable">
							<view class="tt">{{item.score}}积分</view>
							<view class="tt">{{item.reward}}佣金</view>
							<view class="tt">{{item.standard}}人团</view>
						</view>
						<view class="money flexRowBetween">
							<view class="flex left">
								<view class="price">{{item.price}}</view>
								<view class="yuan price">{{item.o_price}}</view>
							</view>
							<view class="addbtn">开团</view>
						</view>
						
					</view>
				</view>
			</view>
		</view>
		<view v-if="mainData.length==0" style="text-align: center;margin-top: 50px;">
			<image src="../../static/images/noContent.png" style="width: 300rpx;height: 300rpx;"></image>
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
				wx_info:{},
				scrollTop: 100,
				index: 0,
				current:1,
				labelData: [
					"../../static/images/home-banner.png",
					"../../static/images/home-banner.png",
				],
				indProDate:[
					{},{},{},{}	
				],
				mainData:[],
				getBefore:{},
				category_id:57,
				user_no:''
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);	
			self.user_no = uni.getStorageSync('user_info').user_no;
			const callback = (res) =>{
				self.$Utils.loadAll(['getMainData','getLabelData'], self);	
			};
			self.$Token.getProjectToken(callback,{refreshToken:true})	
		},
		
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		methods: {
			
			change(current) {
				const self = this;
				if(current!=self.current){
					self.current = current;
					if(self.current==1){
						self.getBefore = {};
						self.category_id = 57
					}else{
						self.getBefore = {
							caseData: {
								tableName: 'Log',
								searchItem: {
									type:['in',[6]],
									user_no:['in',[uni.getStorageSync('user_info').user_no]]
								},
								middleKey: 'id',
								key: 'relation_id',
								condition: 'in',
							},
						};
						self.category_id = ''
					};
					self.getMainData(true)
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
					type:1,
					level:uni.getStorageSync('user_info').level
				};
				if(self.category_id!=''){
					postData.searchItem.category_id = self.category_id
				};
				if(JSON.stringify(self.getBefore)!='{}'){
					postData.getBefore = self.$Utils.cloneForm(self.getBefore);
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			getLabelData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					title:'店铺轮播'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelData = res.info.data[0]
					}
					self.$Utils.finishFunc('getLabelData');
				};
				self.$apis.labelGet(postData, callback);
			},
		},
	};
</script>

<style>
	@import "../../assets/style/page.css";

	page {padding-bottom: 60rpx;}
</style>
