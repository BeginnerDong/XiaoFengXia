<template>
	<view>
		<city-select
			@cityClick="cityClick"
			:formatName="formatName"
			:activeCity="activeCity"
			:hotCity="hotCity"
			:obtainCitys="obtainCitys"
			:isSearch="true"
			ref="citys"
		></city-select>
	</view>
</template>

<script>
import citys from './citys.js'
console.log(citys.length)
import citySelect from '@/components/city-select/city-select.vue'
export default {
	data() {
		return {
			//需要构建索引参数的名称（注意：传递的对象里面必须要有这个名称的参数）
			formatName: 'title',
			//当前城市
			activeCity: {
				id: 1,
				title: '南京市'
			},
			//热门城市
			hotCity: [
				{
					id: 0,
					title: '南京市'
				},
				{
					id: 1,
					title: '南京市'
				}
			],
			//显示的城市数据
			obtainCitys: [
				
			]
		}
	},
	components: {
		citySelect
	},
	
	onLoad() {
		const self = this;
		self.$Utils.loadAll(['getMainData'], self);
	},
	
	methods: {
		
		
		getMainData() {
			const self = this;
			const postData = {};
			postData.searchItem = {
				type:3,
				parentid:['>',0]
			};
			const callback = (res) => {
				if (res.info.data.length > 0) {
					self.obtainCitys = res.info.data;
				}
				self.$Utils.finishFunc('getMainData');
			};
			self.$apis.labelGet(postData, callback);
		},
		
		
		cityClick(item) {
			const self = this;
			console.log(item);
			uni.setStorageSync('city',item.title);
			setTimeout(function() {
				uni.navigateBack({
					delta:1
				})
			}, 500);
		}
	}
}
</script>

<style></style>
