<template>
	<view>
		
		<view class="detail_join" style="padding: 0rpx 4%;">
			<view class="twolist">
				<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index">
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
					<button type="button" style="width: 100%; margin:20rpx 0;" @click="goBuy()">去拼单</button>
				</view>
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
				is_show:false,
				pintuanList:[
					{},{},{},{},{},{},{},{}
				],
				mainData:[],
				product_no:'',
				countDownList:[],
				endTimeList:[],
				chooseGroupData:{},
				chooseGroupIndex:-1
			}
		},
		
		onLoad(options) {
			const self = this;
			self.product_no = options.product_no;
			if(options.leader_no){
				self.leader_no = options.leader_no
			};
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			goBuy() {
				const self = this;
				
				if(JSON.stringify(self.chooseGroupData) != '{}'){
					uni.setStorageSync('group_no', self.chooseGroupData.group_no)
				}else{
					return
				}
				if(self.leader_no){
					self.$Router.navigateTo({
						route: {
							 path: '/pages/confirmOrder/confirmOrder?type=2&leader_no='+self.leader_no
						}
					})
				}else{
					self.$Router.navigateTo({
						route: {
							path: '/pages/confirmOrder/confirmOrder?type=2'
						}
					})
				}
			},
			
			chooseGroup(index){
				const self = this;
				self.is_show = true;
				self.chooseGroupIndex = index;
				self.chooseGroupData = self.mainData[index];
				console.log('self.countDownList',self.chooseGroupIndex)
				console.log('self.countDownList',self.countDownList)
			},
			
			goPinDan(){
				const self = this;
				self.is_show = !self.is_show
			},
			
			getMainData() {
				const self = this;
				console.log('852369')
				var now = Date.parse(new Date());
				const postData = {};
				postData.tokenFuncName = 'getProjectToken'
				postData.searchItem = {
					user_type:0,
					//order_step: 4,
					pay_status: 1,
					product_no:self.product_no ,
					group_leader:'true',
					invalid_time:['>',now]
				};
				postData.getAfter = {
					groupMember:{
					  tableName:'Order',
					  middleKey:'group_no',
					  key:'group_no',
					  searchItem:{
					    status:1,
						pay_status:1
						//group_leader:'false'
					  },
					  condition:'='
					},
					user:{
					  tableName:'User',
					  middleKey:'user_no',
					  key:'user_no',
					  searchItem:{
					    status:1,
					  },
					  condition:'='
					}
				};
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData.push.apply(self.mainData,res.info.data);
						for (var i = 0; i < self.mainData.length; i++) {
							self.endTimeList.push({
								actEndTime: self.mainData[i].invalid_time
							})
							self.mainData[i].lessNum = parseInt(self.mainData[i].standard) -  parseInt(self.mainData[i].groupMember.length)
						}
					}
					self.countDown();
					self.$Utils.finishFunc('getMainData');
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
				self.countDownList=countDownArr;
				
				setTimeout(this.countDown, 1000);
			},
		},
	};
</script>

<style>
	@import "../../assets/style/page.css";

	page {
		padding-bottom: 140rpx;
	}
	

</style>
