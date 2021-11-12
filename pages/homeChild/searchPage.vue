<template>
	<view class="content">
		<image src="@/static/image/homebgPic.png" class="homebg-img" mode="aspectFit" />
		<view class="header">
			<image src="@/static/icon/backIcon.png" class="back-img" mode="aspectFit" @click="backToHome" />
			<text class="header-name">搜索</text>
		</view>
		<view class="search-content">
			<uni-easyinput prefixIcon="search" v-model="searchName" placeholder="搜索最具人气的餐厅/酒店" @input="inputChange"
				:styles="styles" :placeholderStyle="placeholderStyle" />
			<text v-if="resultList.length === 0" @click="searchShopByName">搜索</text>
		</view>
		<view class="result-content">
			<view v-if="resultList && resultList.length > 0" class="result">
				<shop-detail v-for="(item,index) in resultList" :shopDetail="item" :key="index" showType="vertical" />
			</view>
			<view v-else class="recommend">
				<view>热门搜索</view>
				<view>
					<text v-for="(item, index) in popularShopList" :key="index"
						@click="searchSelectedShop(item)">{{item.shopName}}</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import testShopPic3 from "@/static/testPic/testShopPic3.png";
	import testShopImg3 from "@/static/testPic/testShopImg3.png";
	// ************TODO:上面的是开发引入的测试图片，正式使用时需删除**************
	import shopDetail from "@/components/shopDetail.vue";
	export default {
		name: 'searchPage',
		components: {
			shopDetail
		},
		data() {
			return {
				placeholderStyle: "color:#97A4DB;font-size:30rpx",
				styles: {
					color: '#FFFFFF',
					borderColor: '#707070',
				},
				searchName: '',
				popularShopList: [],
				resultList: []
			}
		},
		onShow() {
			//TODO: 每次进入搜索页面都要重新获取热搜推荐的店铺列表数据
			this.popularShopList = [{
				shopId: 9527,
				shopName: '肯德基'
			}, {
				shopId: 3329,
				shopName: '悦景酒店'
			}, {
				shopId: 4567,
				shopName: '味悦餐厅'
			}, {
				shopId: 2021,
				shopName: '胡桃里酒馆'
			}, {
				shopId: 8547,
				shopName: '星巴克'
			}, {
				shopId: 3695,
				shopName: '泰悦澜'
			}];
		},
		methods: {
			backToHome() {
				uni.navigateBack();
			},
			inputChange() {
				if (!this.searchName) {
					this.resultList = [];
				}
			},
			searchShopByName() {
				//TODO: 根据输入的名称获取对应的店铺数据(是否支持模糊搜索？？？)
				this.resultList = [{
					verticalImg: testShopImg3,
					gridImg: testShopPic3,
					shopType: 0,
					shopName: '肯德基',
					discount: 5,
					describe: '限时5折优惠券，全场通用！',
				}];

			},
			searchSelectedShop(item) {
				console.log('选中的店铺是' + item.shopName);
				this.searchName = item.shopName;
				//TODO: 根据选中的词条，搜索相关店铺数据(是否支持模糊搜索？？？)
				this.resultList = [{
					verticalImg: testShopImg3,
					gridImg: testShopPic3,
					shopType: 0,
					shopName: '肯德基',
					discount: 5,
					describe: '限时5折优惠券，全场通用！',
				}];
			}
		}
	}
</script>

<style scoped lang="scss">
	.content {
		position: relative;

		.homebg-img {
			width: 750rpx;
			height: 600rpx;
			position: absolute;
		}

		.header {
			position: relative;
			display: flex;
			align-items: center;
			height: 60rpx;
			padding-top: 50rpx;
			margin-bottom: 40rpx;
			justify-content: center;

			.back-img {
				width: 48rpx;
				height: 48rpx;
				position: absolute;
				left: 20rpx;
			}

			.header-name {
				color: #FFFFFF;
				font-size: 36rpx;
			}
		}

		.search-content {
			padding: 0 30rpx;
			display: flex;
			align-items: center;

			text {
				font-size: 30rpx;
				color: #FFFFFF;
				z-index: 1;
				margin-left: 30rpx;
			}

			uni-easyinput {
				flex: 1;
			}

			/deep/ .uni-easyinput__content {
				background-color: #0C1745;
				border-radius: 38rpx;
				height: 76rpx;
			}
		}

		.result-content {
			margin-top: 20rpx;
			background-color: #131F54;
			height: calc(100vh - 246rpx);
			position: relative;
			overflow: scroll;

			.result {
				padding-top: 20rpx;
			}

			.recommend {
				color: #FFFFFF;

				view {
					&:first-child {
						font-size: 36rpx;
						margin-left: 30rpx;
						padding-top: 16rpx;
					}

					&:last-child {
						font-size: 28rpx;
						padding-right: 50rpx;
						display: flex;
						flex-wrap: wrap;

						text {
							margin: 20rpx 0 0 50rpx;
						}
					}
				}
			}
		}

	}
</style>
