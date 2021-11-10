<template>
	<view class="content">
		<swiper class="swiper" autoplay :interval="2000" :duration="500">
			<swiper-item v-for="(item, index) in bannerList" :key="index">
				<view class="swiper-item">
					<image :src="item.url" class="swiper-item-img" mode="scaleToFill" />
				</view>
			</swiper-item>
		</swiper>
		<uni-easyinput prefixIcon="search" v-model="value" placeholder="搜索最具人气的餐厅/酒店" @iconClick="iconClick"></uni-easyinput>
	</view>
</template>

<script>
	import bannerOne from "@/static/testPic/bannerOne.png";
	import bannerTwo from "@/static/testPic/bannerTwo.png";
	import homebgPic from "@/static/image/homebgPic.png";
	export default {
		data() {
			return {
				bannerList: [{
						url: bannerOne
					},
					{
						url: bannerTwo
					}
				]
			}
		},
		onLoad() {

		},
		methods: {

		}
	}
</script>

<style scoped lang="scss">
	.content {
		.swiper {
			height: 360rpx;
			line-height: 360rpx;

			.swiper-item {
				display: block;
				height: 100%;
				text-align: center;

				.swiper-item-img {
					height: 100%;
					width: 690rpx;
				}
			}
		}

	}
</style>
