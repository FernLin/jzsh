<template>
	<view :class="['shop-detail-content', showType === 'vertical' ? 'padding-vertical' : 'padding-grid']">
		<image :src="showType === 'vertical' ? shopDetail.verticalImg : shopDetail.gridImg"
			:class="showType === 'vertical' ? 'shop-img-vertical' : 'shop-img-grid'" mode="scaleToFill" />
		<view :class="showType === 'vertical' ? 'shop-msg-vertical' : 'shop-msg-grid'">
			<view class="main-title">{{shopDetail.shopName}}{{shopDetail.discount}}折起</view>
			<view class="describe">{{shopDetail.describe}}</view>
			<view :class="['default-btn', shopDetail.shopType ? 'self-checkout-btn' : 'order-online-btn']"
				@click="goto">
				{{shopDetail.shopType ? '自助结账' : '在线点餐'}}
				<text :class="['triangle-default', {'self-checkout-triangle':shopDetail.shopType}]" />
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: 'shopDetailVertical',
		props: {
			shopDetail: {
				type: Object,
				default () {
					return {};
				}
			},
			showType: {
				type: String,
				default: '',
			}
		},
		methods: {
			goto() {
				console.log(this.shopDetail.shopType ? '自助结账' : '在线点餐');
			}
		}
	}
</script>

<style scoped lang="scss">
	.padding-vertical {
		padding: 0 30rpx;
	}

	.padding-grid {
		padding: 0 10rpx;
	}

	.shop-detail-content {
		margin-top: 20rpx;
		position: relative;

		.shop-img-vertical {
			width: 690rpx;
			height: 275rpx;
		}

		.shop-img-grid {
			width: 350rpx;
			height: 375rpx;
		}

		.shop-msg-vertical {
			position: absolute;
			z-index: 2;
			bottom: 16rpx;
			left: 56rpx;

			.main-title {
				color: #FFFFFF;
				font-size: 36rpx;
			}

			.describe {
				color: #FFFFFF;
				font-size: 24rpx;
			}
		}

		.shop-msg-grid {
			width: 350rpx;
			position: absolute;
			z-index: 2;
			bottom: 20rpx;
			display: flex;
			align-items: center;
			flex-direction: column;

			.main-title {
				color: #FFFFFF;
				font-size: 36rpx;
			}

			.describe {
				color: #FFFFFF;
				font-size: 24rpx;
			}
		}
	}

	.default-btn {
		font-size: 26rpx;
		height: 56rpx;
		line-height: 56rpx;
		margin-top: 18rpx;
		border: 1rpx solid #FEF1E0;
		border-radius: 28rpx;
		width: 212rpx;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.self-checkout-btn {
		color: #FFFFFF;
	}

	.order-online-btn {
		color: #653712;
		background: linear-gradient(270deg, #FECC82 0%, #EFCDAD 45%, #FFEADE 100%);
	}

	.triangle-default {
		display: inline-block;
		margin-left: 12rpx;
		width: 0;
		height: 0;
		border-style: solid;
		border-width: 8rpx 0 8rpx 13rpx;
		border-color: transparent transparent transparent #653712;
	}

	.self-checkout-triangle {
		border-color: transparent transparent transparent #FFFFFF;
	}
</style>
