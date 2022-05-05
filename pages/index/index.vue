<template>
	<view >
		<u-swiper :list="slides" name="img_url" height="320"></u-swiper>
		 <view class="u-m-t-30">
			 <u-tabs :list="sortList" :is-scroll="false" :current="currentSort" @change="change"></u-tabs>
		 </view>
		 
		 <u-row gutter="16">
		 	<u-col span="6" v-for="item in goods">
					<navigator class="goods-item">
						<u-image width="100%" height="300rpx" :src="item.cover_url"></u-image>
						<view class="title u-line-1">{{item.title}}</view>
						<view class="u-flex u-row-between">
							<view class="price">￥ {{item.price}}</view>
							<view class="sales">销量:{{item.sales}}</view>
						</view>
					</navigator>
		 	</u-col>
		 </u-row>
		
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
				page : 1
				
			}
		},
		 onLoad() {	
			this.getData()
		},
		methods: {
			change(index) {
				console.log(index),
				this.currentSort = index;
			},
			//
			async getData(){
				const params = {
					page: this.page
				}
				const res = await this.$u.api.index(params)
				console.log(res)
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

<style lang="scss" scoped>
	.goods-item{
		padding: 40rpx;
		margin-top: 30rpx;
		box-shadow: 0 12rpx 20rpx 0 rgba(0, 0, 0, .1);
		.title{
			margin: 10rpx 0;
			font-weight: 500;
			font-size: 32rpx;	
		}
		.price{
			color: red;
		}.sales{
			color: #888;
		}
		
		
    }			
</style>
