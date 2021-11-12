<template>
	<view class="content">
		<view class="area">
			<image src="@/static/image/homebgPic.png" class="homebg-img" mode="aspectFit" />
			<view class="header">
				<image src="@/static/icon/scanIcon.png" class="scan-img" mode="aspectFit" />
				<text class="header-name">{{mpName}}</text>
			</view>
			<swiper class="swiper" autoplay :interval="2000" :duration="500">
				<swiper-item v-for="(item, index) in bannerList" :key="index">
					<view class="swiper-item">
						<image :src="item.url" class="swiper-item-img" mode="scaleToFill" />
					</view>
				</swiper-item>
			</swiper>
			<view v-if="billList && billList.length > 0" class="notice-contnent">
				<view>
					<image src="../../static/icon/noticeIcon.png" mode="scaleToFill" class="notice-img" />
					<view class="notice-word">
						<view>Hi，{{mpName}}提醒您：</view>
						<view>您有{{billList.length}}笔账单已生成，请及时支付...</view>
					</view>
				</view>
				<view :class="['default-btn', 'bill-btn']" @click="checkBillDetail">
					点击进入
					<text class="triangle-default" />
				</view>
			</view>
			<view class="area-input">
				<uni-easyinput prefixIcon="search" v-model="value" placeholder="搜索最具人气的餐厅/酒店" @iconClick="iconClick"
					:styles="styles" :placeholderStyle="placeholderStyle" />
			</view>

		</view>
		<view class="bt-content">
			<view @click="jumpPage('delicacy')">
				<image src="@/static/icon/btDelicacyIcon.png" mode="aspectFit" />
				<text>附近美食</text>
			</view>
			<view @click="jumpPage('dinner')">
				<image src="@/static/icon/btDinnerIcon.png" mode="aspectFit" />
				<text>聚餐宴请</text>
			</view>
			<view @click="jumpPage('snack')">
				<image src="@/static/icon/btSnackIcon.png" mode="aspectFit" />
				<text>精品小吃</text>
			</view>
			<view @click="jumpPage('shop')">
				<image src="@/static/icon/btShopIcon.png" mode="aspectFit" />
				<text>热推好店</text>
			</view>
		</view>
		<view class="shop-content">
			<view class="shop-content-header">
				<text class="header-name">精选商家</text>
				<image :src="showType === 'vertical' ? showGridIcon : showVerticalIcon" class="show-img"
					@click="switchShowType" mode="aspectFit" />
			</view>
			<view class="shop-list">
				<shop-detail-vertical v-for="(item,index) in shopList" :shopDetail="item" :key="index"
					:showType="showType" />
			</view>
		</view>
	</view>
</template>

<script>
	import bannerOne from "@/static/testPic/bannerOne.png";
	import bannerTwo from "@/static/testPic/bannerTwo.png";
	import testShopImg1 from "@/static/testPic/testShopImg1.png";
	import testShopImg2 from "@/static/testPic/testShopImg2.png";
	import testShopImg3 from "@/static/testPic/testShopImg3.png";
	import testShopImg4 from "@/static/testPic/testShopImg4.png";
	import testShopImg5 from "@/static/testPic/testShopImg5.png";
	import testShopPic1 from "@/static/testPic/testShopPic1.png";
	import testShopPic2 from "@/static/testPic/testShopPic2.png";
	import testShopPic3 from "@/static/testPic/testShopPic3.png";
	import testShopPic4 from "@/static/testPic/testShopPic4.png";
	import testShopPic5 from "@/static/testPic/testShopPic5.png";
	// ************TODO:上面的是开发引入的测试图片，正式使用时需删除**************
	import showGridIcon from "@/static/icon/showGridIcon.png";
	import showVerticalIcon from "@/static/icon/showVerticalIcon.png";
	import shopDetailVertical from "@/components/shopDetailVertical.vue";
	export default {
		components: {
			shopDetailVertical
		},
		data() {
			return {
				showGridIcon: showGridIcon,
				showVerticalIcon: showVerticalIcon,
				placeholderStyle: "color:#97A4DB;font-size:30rpx",
				styles: {
					color: '#FFFFFF',
					borderColor: '#707070',
				},
				mpName: "", // 小程序名称
				bannerList: [],
				billList: [],
				showType: 'vertical', // 商店展示类型
				shopList: [{ // 商家信息数据列表（TODO:当前字段为前端开发字段，正式数据字段待前后端统一）
					verticalImg: testShopImg1,
					gridImg: testShopPic1,
					shopType: 0, // 商家类型（0：在线点餐 1：自助结账）
					shopName: '悦景酒家', // 商家名称
					discount: 8, // 商家优惠折扣
					describe: '限时8折优惠券，全场通用！', // 商家折扣信息描述
				}, {
					verticalImg: testShopImg2,
					gridImg: testShopPic2,
					shopType: 1,
					shopName: '阿姆源',
					discount: 8,
					describe: '限时8折优惠券，全场通用！',
				}, {
					verticalImg: testShopImg3,
					gridImg: testShopPic3,
					shopType: 0,
					shopName: '肯德基',
					discount: 5,
					describe: '限时5折优惠券，全场通用！',
				}, {
					verticalImg: testShopImg4,
					gridImg: testShopPic4,
					shopType: 1,
					shopName: '泰悦澜',
					discount: 8,
					describe: '限时8折优惠券，全场通用！',
				}, {
					verticalImg: testShopImg5,
					gridImg: testShopPic5,
					shopType: 0,
					shopName: '奈雪の茶',
					discount: 8,
					describe: '限时8折优惠券，全场通用！',
				}]
			}
		},
		onReady() {
			this.mpName = getApp().globalData.mpName;
			// TODO: 获取轮播图数据
			this.bannerList = [{
				url: bannerOne
			}, {
				url: bannerTwo
			}];
			console.log('测试ready');
		},
		onShow() {
			//TODO: 每次展示当前页面的时候都要访问接口，获取当前用户是否有待处理的账单
			this.billList = [{
				id: 1,
				shopName: '肯德基',
				billId: 9527
			}, {
				id: 2,
				shopName: '奈雪の茶',
				billId: 2021
			}];
			console.log('获取当前用户是否有待处理账单');
		},
		methods: {
			checkBillDetail() {
				//TODO: 跳转账单详情页
				console.log('跳转至账单详情页！');
			},
			jumpPage(areaName) {
				console.log('跳转' + areaName);
				//TODO: 跳转至不同页面
			},
			switchShowType() {
				this.showType = this.showType === 'vertical' ? 'grid' : 'vertical';
			}
		}
	}
</script>

<style scoped lang="scss">
	.content {
		background-color: #131F54;

		.area {
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

				.scan-img {
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

			.notice-contnent {
				position: relative;
				display: flex;
				align-items: center;
				justify-content: space-between;
				height: 98rpx;
				background: linear-gradient(90deg, #F8D7B8 0%, #EFCDAD 100%);
				border-radius: 20rpx;
				margin: 20rpx 30rpx 0;
				padding: 0 12rpx;
				color: #653712;
				
				view {
					&:first-child {
						display: flex;
						align-items: center;
					}
				}

				.notice-img {
					width: 40rpx;
					height: 48rpx;
				}

				.notice-word {
					margin-left: 16rpx;
					view {
						&:first-child {
							font-size: 28rpx;
						}

						&:last-child {
							font-size: 24rpx;
						}
					}
				}

				.bill-btn {
					background-color: #FFFFFF;
					border: 8rpx solid #E6B98F;
					width: 140rpx;
					height: 60rpx;
					border-radius: 40rpx;
					font-size: 24rpx;
				}

				.triangle-default {
					margin-left: 2rpx;
				}
			}

			.area-input {
				margin-top: 20rpx;
				padding: 0 30rpx;

				/deep/ .uni-easyinput__content {
					background-color: #0C1745;
					border-radius: 38rpx;
					height: 76rpx;
				}
			}
		}

		.bt-content {
			display: flex;
			justify-content: space-between;
			padding: 30rpx 42rpx 24rpx;

			view {
				display: flex;
				flex-direction: column;
				align-items: center;
				color: #97A4DB;
				font-size: 28rpx;

				image {
					width: 98rpx;
					height: 98rpx;
					margin-bottom: 8rpx;
				}
			}
		}

		.shop-content {
			.shop-content-header {
				display: flex;
				align-items: center;
				justify-content: space-between;
				padding: 0 40rpx;

				.header-name {
					color: #FFFFFF;
					font-size: 36rpx;
				}

				.show-img {
					width: 48rpx;
					height: 48rpx;
				}
			}

			.shop-list {
				display: flex;
				flex: 1;
				flex-wrap: wrap;
			}
		}
	}
</style>
