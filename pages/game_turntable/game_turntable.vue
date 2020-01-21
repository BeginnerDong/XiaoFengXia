<template>  
    <view class=" pdlr4">  

		<view class="tit" style=""  @click="Router.navigateTo({route:{path:'/pages/game_turntable_rule/game_turntable_rule'}})">
			<view class="name">活动规则</view>
		</view> 

		<view class="main">  
			<view class="canvas-container">  
				<image :src="imgUrl"	style="width: 580rpx;height: 580rpx;"></image>
				<view :animation="animationData" class="canvas-content" >  
					<view class="canvas-line">  
					    <view class="canvas-litem" v-for="(item,index1) in awardsList" :key="index1" :style="[{transform:'rotate('+item.lineTurn+')'}]"></view>  
					</view>  

					<view class="canvas-list">  
					    <view class="canvas-item" v-for="(iteml,index2) in awardsList" :key="index2">  
							<view class="canvas-item-text" :style="[{transform:'rotate('+iteml.turn+')'}]">  
								<text>{{iteml.award}}</text>  
								<image class="canvas-item-text-img" src="../../static/images/icon8.png"></image>  
							</view>  
					    </view>  
					</view>  
				</view>  

				<view @tap="Utils.stopMultiClick(playReward)" class="canvas-btn"></view> 
			</view>  
		</view>  
		
		<view class="submitbtn">
			<button class="hei" type="button">你还有{{5-userInfoData.todayScore.count}}次机会</button>
		</view>
		<view class="submitbtn" style="margin-top: 50rpx;">
			<button type="button"  @click="Router.navigateTo({route:{path:'/pages/game_turntable_winRecord/game_turntable_winRecord'}})">中奖纪录</button>
		</view>
		
    </view>  
</template>
<script>  
    export default {  
		data() {  
			return {  
				Utils:this.$Utils,
				Router:this.$Router,
				showView: false,
				awardsConfig:{  
					chance: true,  
					awards: [  
						{ index: 0, name: '谢谢参与',type:1},
						/* { index: 0, name: '10元红包',type:0},
						{ index: 1, name: '谢谢参与',type:1},
						{ index: 2, name: '50元红包',type:0},
						{ index: 3, name: '谢谢参与',type:1},
						{ index: 4, name: '100元话费',type:0},
						{ index: 5, name: '谢谢参与',type:1},
						{ index: 6, name: '20元红包',type:0},
						{ index: 7, name: '谢谢参与',type:1},
						{ index: 8, name: '888',type:0},
						{ index: 999, name: '999',type:1} */
					]  
				},  
				awardsList: {},  
				animationData: {},  
				btnDisabled: '',  
				chishu:2,
				mainData:[],
				imgUrl:'',
				userInfoData:{},
				awardIndex:''
			};  
		},  
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData','getUserInfoData'], self);
		},
		
		onReady: function (e) {   
			uni.showShareMenu({  
				withShareTicket: true  
			});  
		},  
		
		methods: {  
			
			getUserInfoData() {
				const self = this;
				var dayStart = new Date(new Date().setHours(0, 0, 0, 0)).getTime() / 1000;
				var nowTime = Date.parse(new Date()) / 1000;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				postData.getAfter = {
					todayScore: {
						tableName: 'Log',
						searchItem: {
							create_time:['between',[dayStart,nowTime]],
							status:1,
							type:4,
							user_no:wx.getStorageSync('user_info').user_no,
							relation_table:'product'
						},
						middleKey: 'user_no',
						key: 'user_no',
						condition: 'in',
						compute:{						  
						  count:[
						    'count',
						    'count',
						    {
						      create_time:['between',[dayStart,nowTime]],
						      status:1,
						      type:4,
						      user_no:wx.getStorageSync('user_info').user_no,
						      relation_table:'product'
						    }
						  ]
						},
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0];	
					}
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			getMainData() {
				const self = this;
				
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type:4
				};
				postData.paginate  = {
					count: 0,
					currentPage: 1,
					is_page: true,
					pagesize: 10
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						for (var i = 0; i < self.mainData.length; i++) {
							self.awardsConfig.awards.push(
								{ index: i+1, name: self.mainData[i].title,type:0,id:self.mainData[i].id,product:self.mainData[i]},
							)
						}
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					self.imgUrl = '../../static/images/game-icon-'+self.awardsConfig.awards.length+'.png'
					console.log('self.awardsConfig.awards',self.awardsConfig.awards)
					this.drawAwardRoundel();  
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			logAdd(type) {
				const self = this;	
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data  = {
					type: 4,
					relation_table:'product',
					trade_info:'抽奖记录',
					user_no:uni.getStorageSync('user_info').user_no,
					thirdapp_id:2,
				};
				const callback = (res) => {
					if(res.solely_code==100000){
						uni.setStorageSync('canClick',false);
						if(type==0){
							uni.showModal({
								title: '恭喜',  
								content: '获得' + (self.awardsConfig.awards[self.awardIndex].name),  
								showCancel: false  
							});  
							this.btnDisabled= '';
						}else{
							uni.showModal({
								title: '很遗憾',  
								content: '没中奖 谢谢参与',  
								showCancel: false  
							});  
							this.btnDisabled= '';  
						}
						
						self.getUserInfoData()
					}else{
						uni.setStorageSync('canClick',false);
					}
				};
				self.$apis.logAdd(postData, callback);
			},

			//画抽奖圆盘  
			drawAwardRoundel: function () {			   
					var awards = this.awardsConfig.awards;  
					var awardsList = [];  
					var turnNum = 1 / awards.length*360;  // 文字旋转 turn 值  

					// 奖项列表  
					for (var i = 0; i < awards.length; i++) {  
					  awardsList.push({ turn: i * turnNum + 'deg', lineTurn: i * turnNum + turnNum / 2 + 'deg', award: awards[i].name });  
					}  

					this.btnDisabled = this.awardsConfig.chance ? '' : 'disabled';  
					this.awardsList = awardsList;  
			},

			//发起抽奖  
			playReward: function () {  
				const self = this;
				uni.setStorageSync('canClick',false);
				if (self.userInfoData.todayScore.count >= 5) {  
					uni.setStorageSync('canClick',true);
					uni.showToast({  
						title:'抽奖次数已经用完',  
						icon:'none'  
					})  
					return  
				};
				var awardTest = Math.round(Math.random()*10);
				if(awardTest>6){
					console.log('self.awardsConfig.awards.length',self.awardsConfig.awards.length)
					self.awardIndex = Math.ceil(Math.random()*(self.awardsConfig.awards.length-1));
				}else{
					self.awardIndex = 0;
				};
				console.log('awardTest',awardTest);
				
				//中奖index
				var awardsNum = this.awardsConfig.awards;  
				//var self.awardIndex = Math.round(Math.random()*(awardsNum.length-1));//随机数  
				var runNum = 10;//旋转10周  
				var duration = 3000;//时长  

				// 旋转角度  
				this.runDeg = this.runDeg || 0;  
				this.runDeg = this.runDeg + (360 - this.runDeg% 360) + (360 * runNum - self.awardIndex * (360 / awardsNum.length))
				//创建动画  
				var animationRun = uni.createAnimation({  
				  duration: duration,  
				  timingFunction: 'ease'  
				})  
				animationRun.rotate(this.runDeg).step();  
				this.animationData= animationRun.export();  
				this.btnDisabled= 'disabled';  

				//中奖提示  
				var awardsConfig = this.awardsConfig;  
				var awardType = awardsConfig.awards[self.awardIndex].type;     
				this.chishu = this.chishu - 1;  
				if (awardType == 0) {
					setTimeout(function () {
						var orderList = {
							product: [{
								id: awardsConfig.awards[self.awardIndex].id,
								count: 1,
								product: awardsConfig.awards[self.awardIndex].product
							}],
							type: awardsConfig.awards[self.awardIndex].product.type,
						};
						const postData = self.$Utils.cloneForm(orderList)
						postData.tokenFuncName = 'getProjectToken';
						postData.data = {
							pay_status:1
						};
						const callback = (res) => {
							if (res && res.solely_code == 100000) {
							
									setTimeout(function () {
										self.logAdd('0')
									}.bind(this), duration);  
									
								
							} else {		
								uni.setStorageSync('canClick', true);
								uni.showToast({
									title: res.msg,
									duration: 2000
								});
							};		
						};
						self.$apis.addOrder(postData, callback);
					}.bind(this), duration); 
					
				}else{
					setTimeout(function () {
						self.logAdd('1')
					}.bind(this), duration);  
				}
				
			}  
		}  
    }  
</script>

<style>  
	
.tit{ display: flex; justify-content: flex-end;padding: ;}
.tit .name{font-size: 28rpx; padding: 20rpx;}
.turntable{width: 580rpx; height: 580rpx; margin: 40rpx auto 60rpx auto;}
.turntable .pic{width: 100%; height: 100%; display: block;}

.header{  
    display: flex;  
    flex-direction: column;  
    align-items: center;  
    justify-content: center;  
    height: 100rpx;  
}  
/* 转盘 */  
.canvas-container {  
	margin:60rpx auto 80rpx auto;  
	position: relative;  
	width: 580rpx;  
	height: 580rpx; 
	
}  
.canvas-content {  
  position: absolute;  
  top:0;  
  left:0; 
   
  z-index: 1;  
  display: block;  
  width: 100%;  
  height:100%;  
  border-radius: inherit;  
  background-clip: padding-box;   
}  
.canvas-element {  
  position: relative;  
  z-index: 1;  
  width: inherit;  
  height: inherit;  
  border-radius: 50%;  
}  
.canvas-list {  
  position: absolute;  
  left: 0;  
  top: 0;  
  width: inherit;  
  height: inherit;  
  z-index: 9999;  
}  
.canvas-item {  
  position: absolute;  
  left: 0;  
  top: 0;  
  width: 100%;  
  height: 100%;  
  color: #222;  
}  
.canvas-item-text {  
  position: relative;  
  display: block;  
  padding-top: 68rpx;  
  margin: 0 auto;  
  text-align: center;  
  -webkit-transform-origin: 50% 290rpx;  
  transform-origin: 50% 290rpx;  
    display: flex;  
    flex-direction: column;  
    align-items: center;  
}  
.canvas-item-text text{  
    font-size:26rpx;  
}  
.canvas-item-text-img{  
    width:50rpx;  
    height:50rpx;  
    padding-top:20rpx;  
}  

/* 分隔线 */  
.canvas-line {  
  position: absolute;  
  left: 50%;  
  top: 50%;  
  transform: translate(-50%,-50%); 
  width: inherit;  
  height: inherit;  
  z-index: 99;  
  width: 85%;
  height: 85%;
  display: none;
}  
.canvas-litem {  
  position: absolute;  
  left: 50%;  
  top:0;  
  transform: translate(-50%,-50%);
  width: 2rpx;  
  height: 100%;  
  background-color:#222;  
  overflow: hidden;  
}  

/**  
* 抽奖按钮  
*/  
.canvas-btn {  
  position: absolute;  
  left: 50%;  
  top: 50%;  
  z-index: 400;  
  width: 110rpx;  
  height: 196rpx;
	transform: translate(-50%,-70%); 
  background: url(../../static/images/game-icon1.png) no-repeat 0 0/100% 100%;
}  

.canvas-btn.disabled {  
  pointer-events: none;  
  background: #b07a7b;  
  color: #ccc;  
}  
.canvas-btn.disabled::after {  
  border-bottom-color: #b07a7b;  
}  
.canvas-btn-table{  
    color:#A83FDB;  
    width:120rpx;  
    text-align: center;  
    position: absolute;  
    left: 240rpx;  
    top:360rpx;  
    font-size:26rpx;  
    background-color: #FFFFFF;  
    opacity: 0.9;  
}  
</style>