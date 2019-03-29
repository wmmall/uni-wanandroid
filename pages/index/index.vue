<template>
	<view>
		<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" :circular="true">
			<swiper-item v-for="(item, index) in bannerData" :key="index">
				<view class="swiper-item" @tap="toDetail(item.url, item.title)">
					<image :src="item.imagePath" class="banner"></image>
					<view class="banner-desc uni-h5">  {{ item.title }}</view>
				</view>
			</swiper-item>
		</swiper>
		<view class="uni-list">
			<block v-for="(item,index) in data" :key="index">
				<view class="uni-list-cell" hover-class="uni-list-cell-hover" @tap="toDetail(item.link, item.title)">
					<view class="uni-triplex-row">
						<view class="uni-triplex-left">
							<view class="uni-row">
								<text class="uni-text">{{ item.author }}</text>
								<text class="uni-h5">{{ item.niceDate }}</text>
							</view>
							<text class="uni-title uni-ellipsis">{{ item.title }}</text>
							<text class="uni-text-small uni-ellipsis">{{ item.superChapterName }}/{{ item.chapterName }}</text>
						</view>
					</view>
				</view>
			</block>
		</view>
	</view>
</template>

<script>
	let util = require("../../common/util.js");
	
	export default {
		data() {
			return {
				data: [],
				bannerData: []
			}
		},
		onLoad() {
			this.loadData();
			this.loadBannerData();
		},
		methods: {
			loadData() {
				this.page = this.page? this.page : 0;
				uni.showLoading({
					title: '加载中。。'
				})
				uni.request({
					url: 'https://wanandroid.com/article/listproject/'+this.page+'/json',
					success: (res) => {
						if (!res.data.errorCode) {
							if (this.page <= 0) {
								this.data = res.data.data.datas;
							} else {
								if (res.data.data.curPage <= res.data.data.pageCount) {
									this.data.push.apply(this.data, res.data.data.datas);
								}
							}							
						}
					},
					complete: () => {
						uni.stopPullDownRefresh();
						uni.hideLoading();
					}
				})
			},
			
			loadBannerData() {
				uni.request({
					url: 'https://www.wanandroid.com/banner/json',
					success: (res) => {
						this.bannerData = res.data.data;
					}
				})
			},
			
			toDetail(url, title) {
				uni.navigateTo({
					url: '../web/web?url='+url + '&title=' + title,
				});
			}
		},
		onPullDownRefresh() {
			this.page = 0;
			this.loadData();
		},
		
		onReachBottom() {
			this.page++;
			this.loadData();
		}
	}
</script>

<style>
	.uni-triplex-left {
		width: 100%;
	}
	.uni-row {
		display: flex;
		justify-content: space-between;
	}
	swiper {
		height: 450upx;
	}
	.swiper-item {
		display: flex;
		flex-direction: column;
	}
	.banner {
		width: 100%;
		height: 450upx;
	}
	.banner-desc {
		width: 98%;
		height: 60upx;
		margin-top: -60upx;
		color: #fff;
		padding-left: 2%;
		line-height: 60upx;
		background-color: rgba(0, 0, 0, 0.5);
	}
</style>
