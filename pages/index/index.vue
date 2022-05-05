<template>
	<view >
		<u-swiper :list="slides" name="img_url" height="320"></u-swiper>
		 <view class="u-text-center u-m-t-30">
			 <u-tabs :list="sortList" :is-scroll="false" :current="currentSort" @change="change"></u-tabs>
		 </view>
		 
		 <u-row gutter="16" class="u-skeleton">
		 	<u-col span="6" v-for="item in goods.length !== 0 ? goods : [{},{},{},{}]">
					<goods-card :item = "item"></goods-card>
		 	</u-col>
		 </u-row>
		<u-skeleton :loading="loading" :animation="true" bgColor="#FFF"></u-skeleton>
	</view>
</template>

<script>
	export default {
		
		data() {
			return {
				sortList :[
					{name : '默认'},
					{name : '推荐'},
					{name : '热门'},
					{name : '最新'},	
				],
				currentSort : 0,
				slides: [],
				goods:[],
				page : 1,
				loading: true
			}
		},
		 onLoad() {	
			this.getData()
		},
		methods: {
			change(index) {
				console.log(index),
				this.currentSort = index;
				this.goods = []
				this.page = 1
				this.getData()
			},
			
			async getData(){
				this.loading = true
				const params = {
					page: this.page
				}
				if(this.currentSort == 1) params.sales = 1
				if(this.currentSort == 2) params.recommend = 1
				if(this.currentSort == 3) params.new = 1
				const res = await this.$u.api.index(params)
				console.log(res)
				this.loading = false
				this.slides = res.slides
				this.goods = [...this.goods, ... res.goods.data]
			}
		},
		onReachBottom(){
			this.page = this.page + 1
			this.getData()
		}
	}
</script>

<style >
	
</style>
