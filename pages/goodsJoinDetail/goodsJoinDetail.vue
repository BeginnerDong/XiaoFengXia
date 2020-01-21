<template>
	<view>
		<button class="posterBtn" v-if="isVip" open-type='share' id='1'>分享海报</button>
		<button class="posterBtn" v-else open-type='share' id='2'>分享海报</button>
		<!-- <view class="detailxqBan">
			<image :src="mainData.bannerImg&&mainData.bannerImg[0]?mainData.bannerImg[0].url:''" mode=""></image>
		</view> -->
		<view class="detailxqBan">
			<swiper style="width: 100%;height: 100%;" indicator-dots="true" autoplay="true" interval="3000" duration="1000"
			 indicator-active-color="#434343">
				<block v-for="(item,index) in mainData.bannerImg" :key="index">
					<swiper-item class="swiper-item">
						<image :src="item.url" class="slide-image" />
					</swiper-item>
				</block>
			</swiper>
		</view>
		<view class="detail_one">
			<view class="flex">
				<view class="price font14">{{mainData.price}}</view>
				<view class="yuanJia">{{mainData.o_price}}</view>
			</view>
			<view class="flexRowBetween color2 font13 number">
				<view class="">库存：{{chooseSku.stock?chooseSku.stock:''}}</view>
				<view class="">销量：{{mainData.sale_count?mainData.sale_count:''}}</view>
			</view>
			<view class="tit font14">{{mainData.title}}</view>
		</view>
		<view class="f5H10"></view>
		<view class="detal_guige pdlr4 flexRowBetween" @click="showSel">
			<view class="flexRowAround ll">
				<view>规格</view>
				<!-- <view class="yyDate-lis flex" v-if="noSelectSku">
					<view v-for="item in mainData.sku">{{item.title}}</view>
				</view> -->
				<view class=" flex">
					<view>已选:{{chooseSku.title?chooseSku.title:''}}*{{count}}件</view>
				</view>
			</view>
			<view class="rr">
				<image class="arrowR" src="../../static/images/home-icon3.png" mode=""></image>
			</view>
		</view>
		<view class="f5H10"></view>
		<view class="detail_join" v-if="groupData.length>0">
			<view class="flexRowBetween">
				<view class="font14">{{total}}人正在拼团，可直接参与</view>
				<view class="font13 color3" @click="viewMoreGroup">查看更多&gt;</view>
			</view>
			<view class="twolist" v-if="hasOrder">
				<view class="item flexRowBetween" v-for="(item,index) in groupData" v-if="index!=groupData.length-1">
					<view class="ll flex">
						<view class="photo">
							<image :src="item.user&&item.user[0]?item.user[0].headImgUrl:''" mode=""></image>
						</view>
						<view class="tit avoidOverflow">{{item.user&&item.user[0]?item.user[0].nickname:''}}</view>
					</view>
					<view class="rr flexRowBetween">
						<view class="downTime">
							<view class="red font14" v-if="item.lessNum>0">还差{{item.lessNum}}人拼成</view>
							<view class="red font14" v-else>已参团{{item.groupMember.length}}人</view>
							<view class="time">剩余{{countDownList[index].hou}}:{{countDownList[index].min}}:{{countDownList[index].sec}}</view>
						</view>
						<view class="go" @click="chooseGroup(index)">去拼团</view>
					</view>
				</view>
			</view>
			<view class="twolist" v-if="!hasOrder">
				<view class="item flexRowBetween" v-for="(item,index) in groupData">
					<view class="ll flex">
						<view class="photo">
							<image :src="item.user&&item.user[0]?item.user[0].headImgUrl:''" mode=""></image>
						</view>
						<view class="tit avoidOverflow">{{item.user&&item.user[0]?item.user[0].nickname:''}}</view>
					</view>
					<view class="rr flexRowBetween">
						<view class="downTime">
							<view class="red font14" v-if="item.lessNum>0">还差{{item.lessNum}}人拼成</view>
							<view class="red font14" v-else>已参团{{item.groupMember.length}}人</view>
							<view class="time">剩余{{countDownList[index].hou}}:{{countDownList[index].min}}:{{countDownList[index].sec}}</view>
						</view>
						<view class="go" @click="chooseGroup(index)">去拼团</view>
					</view>
				</view>
			
			</view>
		</view>
		<view class="detail_join" v-else>
			<view>暂无拼团，快去开团吧</view>
		</view>
		<view class="f5H10"></view>
		<view class="infor-title">
			<view class="tt">详情介绍</view>
		</view>
		<view class="detailTexCont" style="padding: 20rpx 0;">
			<view class="center">
				<view class="content ql-editor" style="padding: 12px 0;line-height: 0;" v-html="mainData.content">
				</view>
			</view>
		</view>

		<!-- 底部菜单按钮 -->
		<view class="xqbotomBar">
			<view class="left flexRowBetween" style=" width: 32%;">
				<view class="ite" style="width:50;" @click="prev">
					<image src="../../static/images/details=icon1.png" mode=""></image>
					<view>返回</view>
				</view>
				<view class="ite" style="width:50;" @click="addToShop">
					<image src="../../static/images/details=icon2.png" mode=""></image>
					<view>{{hasAdd?'已加入':'加入店铺'}}</view>
				</view>
			</view>
			<view class="payBtn flexRowBetween" style="width: 68%;">
				<view class="item hei" @click="goBuy('1')">
					<view class="price">{{price}}</view>
					<view class="name">单独购买</view>
				</view>
				<view class="item yellow" @click="goBuy('2')">
					<view class="price">{{groupPrice}}</view>
					<view class="name">开团</view>
				</view>
			</view>
		</view>

		<!-- 拼团弹框 -->
		<view class="PinDanAlert" v-if="is_show">
			<view class="explain center">
				<view class="colseBtn" @click="goPinDan" style="left: auto;right: 10rpx;top: 20rpx;">×</view>
				<view class="tit">参与{{chooseGroupData.user&&chooseGroupData.user[0]?chooseGroupData.user[0].nickname:''}}的拼单</view>
				<view>
					<text class="red" v-if="chooseGroupData.lessNum>0">还剩{{chooseGroupData.lessNum}}个名额。</text>
					<text class="red" v-if="chooseGroupData.lessNum<=0">已参团{{chooseGroupData.groupMember.length}}人。</text>

					{{countDownList[chooseGroupIndex].hou}}:{{countDownList[chooseGroupIndex].min}}:{{countDownList[chooseGroupIndex].sec}}后结束
				</view>
				<view class="flexCenter">
					<view class="photo">
						<image :src="chooseGroupData.user&&chooseGroupData.user[0]?chooseGroupData.user[0].headImgUrl:''"></image>
					</view>
					<view class="photo noPic">
						<image src="../../static/images/who.png"></image>
					</view>
				</view>
				<view class="submitbtn">
					<button type="button" style="width: 100%; margin:20rpx 0;" @click="goBuy('2')">去拼单</button>
				</view>
			</view>
		</view>

		<!-- 加入店铺弹框 -->
		<view class="PinDanAlert" v-if="show_addok">
			<view class="explain center" style="background: none;">

				<view class="addShpPic">
					<image src="../../static/images/details-icon2.png" mode=""></image>
				</view>
				<view class="colseBtn" @click="showAddok" style="top: auto;bottom: -80rpx;border: 1rpx solid #fff; color: #fff;background: none;">×</view>
			</view>
		</view>

		<!-- 选择规格弹框 -->
		<view class="showSel" v-if="is_show1">
			<view class="showSelCont fix">
				<view class="colseBtn" @click="showSel">×</view>
				<view class="ind_proList">
					<view class="item flexRowBetween" style="padding:0rpx 0 20rpx 0;border-bottom: 0;">
						<view class="ll">
							<image :src="mainData&&mainData.mainImg&&mainData.mainImg[0]?mainData.mainImg[0].url:''" mode=""></image>
						</view>
						<view class="rr">
							<view class="avoidOverflow2 title">{{mainData.title}}</view>
							<view class="money">
								<view class="flex left">
									<view class="price">{{chooseSku.group_price}}</view>
								</view>
							</view>

						</view>
					</view>
				</view>
				<view class="yy-title" style="padding: 10px 0;">规格</view>
				<view class="yyDate-lis flexRowBetween" style="flex-wrap: wrap; justify-content: flex-start;">
					<view v-for="(item,index) in mainData.sku" :key="index" :class="[chooseSku.id == item.id?'on':'']" @click="seltSpecs(index)">{{item.title}}</view>
				</view>
				<view class="flexRowBetween" style="padding: 30rpx 0; border-top: 2rpx solid #e7e7e7; border-bottom: 2rpx solid #e7e7e7; margin-top: 10rpx;">
					<view class="yy-title">购买数量</view>
					<view class="numBox" style="position: relative; right: auto; bottom: auto;">
						<view @click="counter('-')">-</view>
						<view class="num">{{count}}</view>
						<view @click="counter('+')">+</view>
					</view>
				</view>
				<view class="submitbtn" style="margin-top: 60rpx;">
					<button class="hei" style="margin-bottom: 0;" type="submit" @click="showSel">确定</button>
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
				showView: false,
				is_show: false,
				show_addok: false,
				is_show1: false,
				mainData: {},
				chooseSku: {},
				noSelectSku: true,
				count: 1,
				price: '',
				groupPrice: '',
				hasAdd: false,
				isVip: false,
				groupData: [],
				countDownList: [],
				endTimeList: [],
				chooseGroupData: {},
				chooseGroupIndex: -1,
				total: 0,
				hasOrder:false
			}
		},

		onLoad(options) {
			const self = this;
			self.id = options.id;
			if (options.user_no) {
				self.user_no = options.user_no;
				self.isVip = true
			};
			if (options.leader_no) {
				self.leader_no = options.leader_no
			};
			if (options.order_id) {
				self.order_id = options.order_id;
				self.hasOrder = true
			};
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData', 'getUserInfoData'], self);
		},

		onUnload() {
			const self = this;
			//清除计时器  即清除setInter
			clearTimeout(self.countDown())
		},

		methods: {

			getUserInfoData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken'
				postData.searchItem = {
					user_no: uni.getStorageSync('user_info').user_no
				};
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.userInfoData = res.info.data[0];
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},

			addToShop() {
				const self = this;
				if (self.hasAdd) {
					self.$Utils.showToast('商品已在店铺', 'none');
					return
				};
				if (self.userInfoData.level >= 3 && self.userInfoData.check_status == 2) {
					const postData = {};
					postData.data = {
						thirdapp_id: 2,
						type: 6,
						relation_table: 'product',
						relation_id: self.id,
						user_no: uni.getStorageSync('user_info').user_no,
					};
					postData.tokenFuncName = 'getProjectToken';
					const callback = (res) => {
						if (res.solely_code == 100000) {
							self.show_addok = true;
							self.getMainData()
						} else {
							self.$Utils.showToast(res.msg, 'none');
						}
					};
					self.$apis.logAdd(postData, callback);
				} else {
					self.$Utils.showToast('您暂未拥有店铺', 'none');
				}
			},

			counter(type) {
				const self = this;
				if (type == '+') {
					self.count++
				} else if (type == '-' && self.count > 1) {
					self.count--
				};
				self.countTotalPrice()
			},

			countTotalPrice() {
				const self = this;
				self.price = self.chooseSku.price * self.count.toFixed(2);

				self.groupPrice = self.chooseSku.group_price * self.count.toFixed(2)
			},

			viewMoreGroup() {
				const self = this;
				var orderList = {
					sku: [{
						id: self.chooseSku.id,
						count: self.count,
						sku: self.chooseSku
					}],
					type: self.mainData.type,
				};
				uni.setStorageSync('payPro', orderList);
				if (self.leader_no) {
					self.$Router.navigateTo({
						route: {
						path: '/pages/goodsDetail_pintuan/goodsDetail_pintuan?product_no=' + self.mainData.product_no + '&leader_no=' +self.leader_no
						},

					})
				} else {
					self.$Router.navigateTo({
						route: {
						path: '/pages/goodsDetail_pintuan/goodsDetail_pintuan?product_no=' + self.mainData.product_no
						}
					})
				}
			},

			goBuy(type) {
				const self = this;
				if (JSON.stringify(self.chooseSku) == '{}') {
					self.$Utils.showToast('商品错误', 'none', 1000);
					return;
				};
				var orderList = {
					sku: [{
						id: self.chooseSku.id,
						count: self.count,
						sku: self.chooseSku
					}],
					type: self.mainData.type,
				};
				uni.setStorageSync('payPro', orderList);
				if (JSON.stringify(self.chooseGroupData) != '{}') {
					uni.setStorageSync('group_no', self.chooseGroupData.group_no)
				};
				if (self.leader_no) {
					self.$Router.navigateTo({
						route: {
							path: '/pages/confirmOrder/confirmOrder?type=' + type + '&leader_no=' + self.leader_no
						}
					})
				} else {
					self.$Router.navigateTo({
						route: {
							path: '/pages/confirmOrder/confirmOrder?type=' + type
						}
					})
				}
			},
			//选择商品属性
			seltSpecs(index) {
				const self = this;
				if (self.mainData.sku[index].id != self.chooseSku.id) {
					self.chooseSku = self.mainData.sku[index];
					self.count = 1;
					self.countTotalPrice()
				}

			},

			goPinDan() {
				const self = this;
				self.is_show = !self.is_show;
			},

			chooseGroup(index) {
				const self = this;
				self.is_show = true;
				self.chooseGroupIndex = index;
				self.chooseGroupData = self.groupData[index];
				console.log('self.countDownList', self.chooseGroupIndex)
				console.log('self.countDownList', self.countDownList)
			},

			showSel() {
				const self = this;
				self.is_show1 = !self.is_show1
			},

			showAddok() {
				const self = this;
				self.show_addok = !self.show_addok
			},

			prev() {
				const self = this;
				self.$Router.switchTab({
					route: {
						path: '/pages/index/index'
					}
				})
			},

			onShareAppMessage: function(ops) {
				console.log(ops)
				const self = this;
				if (ops.from === 'button') {
					// 来自页面内转发按钮
					if(ops.target.id==1){
						if(self.groupData.length==0){
							return {
								title: self.mainData.title,
								imageUrl: self.mainData.mainImg[0]&&self.mainData.mainImg[0].url?self.mainData.mainImg[0].url:'',
								desc: '小蜂侠社区团购',
								path: '/pages/goodsJoinDetail/goodsJoinDetail?id=' + self.mainData.id + '&leader_no=' + self.user_no, //点击分享的图片进到哪一个页面
								success: function(res) {
									// 转发成功
									console.log("转发成功:" + JSON.stringify(res));
								},
								fail: function(res) {
									// 转发失败
									console.log("转发失败:" + JSON.stringify(res));
								}
							}	
						}else{
							return {
								title: self.mainData.title,
								imageUrl: self.mainData.mainImg[0]&&self.mainData.mainImg[0].url?self.mainData.mainImg[0].url:'',
								desc: '小蜂侠社区团购',
								path: '/pages/goodsJoinDetail/goodsJoinDetail?id=' + self.mainData.id + '&order_id=' + self.groupData[0].id, //点击分享的图片进到哪一个页面
								success: function(res) {
									// 转发成功
									console.log("转发成功:" + JSON.stringify(res));
								},
								fail: function(res) {
									// 转发失败
									console.log("转发失败:" + JSON.stringify(res));
								}
							}
						}
					}else if(ops.target.id==2){
						return {
							title: self.mainData.title,
							imageUrl: self.mainData.mainImg[0]&&self.mainData.mainImg[0].url?self.mainData.mainImg[0].url:'',
							desc: '小蜂侠社区团购',
							path: '/pages/goodsJoinDetail/goodsJoinDetail?id=' + self.mainData.id, //点击分享的图片进到哪一个页面
							success: function(res) {
								// 转发成功
								console.log("转发成功:" + JSON.stringify(res));
							},
							fail: function(res) {
								// 转发失败
								console.log("转发失败:" + JSON.stringify(res));
							}
						}
					}
					
					
					console.log(ops.target)
				} else {
					return {
						title: self.mainData.title,
						imageUrl: self.mainData.mainImg[0]&&self.mainData.mainImg[0].url?self.mainData.mainImg[0].url:'',
						desc: '小蜂侠社区团购',
						path: '/pages/goodsJoinDetail/goodsJoinDetail?id=' + self.mainData.id, //点击分享的图片进到哪一个页面
						success: function(res) {
							// 转发成功
							console.log("转发成功:" + JSON.stringify(res));
						},
						fail: function(res) {
							// 转发失败
							console.log("转发失败:" + JSON.stringify(res));
						}
					}
					console.log(ops.target)
				}
			},

			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type: 1,
					id: self.id
				};
				postData.getAfter = {
					sku: {
						tableName: 'Sku',
						middleKey: 'product_no',
						key: 'product_no',
						searchItem: {
							status: 1,
						},
						condition: '='
					},

					log: {
						tableName: 'Log',
						middleKey: 'id',
						key: 'relation_id',
						searchItem: {
							status: 1,
							type: 6,
							user_no: ['in', uni.getStorageSync('user_info').user_no]
						},
						condition: '='
					},
				};
				postData.saveFunction = [{
					FuncName: 'viewProduct',
					searchItem: {
						id: self.id
					}
				}];
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						if (self.mainData.sku[0]) {
							self.chooseSku = self.mainData.sku[0];
							self.countTotalPrice();
							self.getGroupData()
						} else {
							self.$Utils.showToast('商品信息错误', 'none');
							setTimeout(function() {
								uni.navigateBack({
									delta: 1
								})
							}, 1500);
						};
						if (self.mainData.log.length > 0) {
							self.hasAdd = true
						};
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},

			getGroupData() {
				const self = this;
				console.log('852369')
				var now = Date.parse(new Date());
				const postData = {};
				postData.tokenFuncName = 'getProjectToken'
				postData.paginate = {
					count: 0,
					currentPage: 1,
					is_page: true,
					pagesize: 5
				};
				postData.searchItem = {
					user_type: 0,
					order_step: 4,
					pay_status: 1,
					product_no: self.mainData.product_no,
					group_leader: 'true',
					invalid_time: ['>', now]
				};
				if(self.leader_no){
					postData.searchItem.leader_no = self.leader_no;
					delete postData.searchItem.order_step
				};
				if(self.user_no){
					postData.searchItem.leader_no = self.user_no;
					delete postData.searchItem.order_step
				};
				postData.getAfter = {
					groupMember: {
						tableName: 'Order',
						middleKey: 'group_no',
						key: 'group_no',
						searchItem: {
							status: 1,
							pay_status: 1
							//group_leader:'false'
						},
						condition: '='
					},
					user: {
						tableName: 'User',
						middleKey: 'user_no',
						key: 'user_no',
						searchItem: {
							status: 1,
						},
						condition: '='
					}
				};
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.groupData.push.apply(self.groupData, res.info.data);
						for (var i = 0; i < self.groupData.length; i++) {
							self.endTimeList.push({
								actEndTime: self.groupData[i].invalid_time
							})
							self.groupData[i].lessNum = parseInt(self.groupData[i].standard) - parseInt(self.groupData[i].groupMember.length)
						};
						if(self.leader_no){
							self.is_show = true;
							self.chooseGroupIndex = self.groupData.length - 1;
							self.chooseGroupData = self.groupData[self.groupData.length - 1];
						}
					};
					self.countDown();
					if (self.order_id) {
						self.getShareOrder()
					};
					self.total = res.info.total;
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.orderGet(postData, callback);
			},

			getShareOrder() {
				const self = this;
				console.log('852369')
				var now = Date.parse(new Date());
				const postData = {};
				postData.tokenFuncName = 'getProjectToken'
				postData.paginate = {
					count: 0,
					currentPage: 1,
					is_page: true,
					pagesize: 5
				};
				postData.searchItem = {
					user_type: 0,
					//order_step: 4,
					pay_status: 1,

					group_leader: 'true',
					invalid_time: ['>', now],
					id: self.order_id
				};
				postData.getAfter = {
					groupMember: {
						tableName: 'Order',
						middleKey: 'group_no',
						key: 'group_no',
						searchItem: {
							status: 1,
							pay_status: 1
							//group_leader:'false'
						},
						condition: '='
					},
					user: {
						tableName: 'User',
						middleKey: 'user_no',
						key: 'user_no',
						searchItem: {
							status: 1,
						},
						condition: '='
					}
				};
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.groupData.push.apply(self.groupData, res.info.data);
						for (var i = 0; i < self.groupData.length; i++) {
							self.endTimeList.push({
								actEndTime: self.groupData[i].invalid_time
							})
							self.groupData[i].lessNum = parseInt(self.groupData[i].standard) - parseInt(self.groupData[i].groupMember.length)
						}
						self.is_show = true;
						self.chooseGroupIndex = self.groupData.length - 1;
						self.chooseGroupData = self.groupData[self.groupData.length - 1];
						console.log('self.countDownList', self.chooseGroupIndex)
						console.log('self.countDownList', self.countDownList)
					} else {
						self.$Utils.showToast('团数据异常', 'none');
					}
					self.countDown();

					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.orderGet(postData, callback);
			},

			timeFormat(param) { //小于10的格式化函数
				const self = this;
				return param < 10 ? '0' + param : param;
			},

			countDown() { //倒计时函数
				// 获取当前时间，同时得到活动结束时间数组
				const self = this;
				let newTime = new Date().getTime();
				let endTimeList = self.endTimeList;
				let countDownArr = [];
				// 对结束时间进行处理渲染到页面
				for (var i = 0; i < self.endTimeList.length; i++) {
					let endTime = self.endTimeList[i].actEndTime;
					let obj = null;
					// 如果活动未结束，对时间进行处理
					if (endTime - newTime > 0) {
						let time = (endTime - newTime) / 1000;
						// 获取天、时、分、秒
						let day = parseInt(time / (60 * 60 * 24));
						let hou = parseInt(time % (60 * 60 * 24) / 3600);
						let min = parseInt(time % (60 * 60 * 24) % 3600 / 60);
						let sec = parseInt(time % (60 * 60 * 24) % 3600 % 60);
						if (day > 0) {
							hou = hou + day * 24
						}
						obj = {
							hou: self.timeFormat(hou),
							min: self.timeFormat(min),
							sec: self.timeFormat(sec)
						}
					} else { //活动已结束，全部设置为'00'
						obj = {
							hou: '00',
							min: '00',
							sec: '00'
						}
					}
					countDownArr.push(obj);

				}
				// 渲染，然后每隔一秒执行一次倒计时函数
				self.countDownList = countDownArr;

				setTimeout(this.countDown, 1000);
			},
		},
	};
</script>

<style>
	@import "../../assets/style/page.css";
	@import "../../assets/style/car.css";

	page {
		padding-bottom: 140rpx;
	}

	.posterBtn {
		position: absolute;
		top: 80rpx;
		right: 0;
		z-index: 2;
		background: #F3D012;
		padding: 0 16rpx;
		border-radius: 30rpx 0 0 30rpx;
		font-size: 20rpx;
		line-height: 44rpx;
	}

	.addShpPic {
		width: 420rpx;
		height: 390rpx;
		margin: 0 auto;
	}

	.addShpPic image {
		width: 100%;
		height: 100%;
	}

	.showSel {
		width: 100%;
		height: 100%;
		background: rgba(0, 0, 0, 0.5);
		position: fixed;
		top: 0;
		right: 0;
		bottom: 0;
		left: 0;
		z-index: 10000;
	}

	.showSelCont {
		padding: 40rpx 4%;
		background: #fff;
	}

	.showSelCont.fix {
		position: fixed;
		bottom: 0;
		left: 0;
		box-sizing: border-box;
		width: 100%;
		border-radius: 20rpx 20rpx 0 0;
	}

	.yy-title {
		font-size: 28rpx;
	}

	.yyDate-lis {
		max-height: 270rpx;
		overflow-y: auto;
	}

	.yyDate-lis view {
		width: 130rpx;
		height: 56rpx;
		font-size: 26rpx;
		color: #666;
		text-align: center;
		line-height: 56rpx;
		background: #f5f5f5;
		margin: 0 20rpx 30rpx 20rpx;
	}

	.yyDate-lis view.on {
		background: #F3D012;
		color: #333;
	}

	.showSel .numBox view {
		background: #f5f5f5;
	}

	.detal_guige {
		padding: 20rpx 4%;
	}

	.detal_guige .ll {
		width: 90%;
		display: flex;
		justify-content: flex-start;
		align-items: center;
	}

	.detal_guige .rr {
		width: 10%;
		display: flex;
		justify-content: flex-end;
	}

	.detal_guige .yyDate-lis view {
		margin: 0 0 0 20rpx;
	}
</style>
