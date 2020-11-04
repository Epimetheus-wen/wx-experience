<template>
	<view class="header">
		<view class="status_bar" :style="'height: calc('+((staHeight*2)+80)+'rpx);'">
			<view class="top_view" :style="'height:'+staHeight+'px;'"></view>
			<view class="top titles">
				<view class="back" @tap="back">
					<image class="img" src="../static/back01_03.png" mode=""></image>
				</view>
				<view class="titleText">{{titleTop}}</view>
			</view>
		</view>
		<view class="geduan" :style="'height: calc('+((staHeight*2)+80)+'rpx);'"></view>
	</view>
</template>

<script>
	export default {
		props: ['titleTop'],
		data() {
			return {
				staHeight: ""
			};
		},
		created() {
			console.log("状态栏高度")
			let that = this
			wx.getSystemInfo({
				success: e => {
					console.log("状态栏高度", e.statusBarHeight)
					that.staHeight = e.statusBarHeight
				}
			})
			wx.showShareMenu({
				withShareTicket: true
			})
		},
		methods: {
			back() {
				uni.navigateBack({
					delta: 1,
					success: () => {
						console.log("ss")
					},
					fail: () => {
						console.log("dd")
						uni.navigateTo({
							url: '/pages/index/index',
							animationType: 'pop-out',
							animationDuration: 200
						});
					}
				});

			}
		}
	}
</script>

<style lang="scss">
	.header {
		.status_bar {
			height: calc(var(--status-bar-height) + 80rpx);
			width: 100%;
			position: fixed;
			top: 0;
			background: #fff;
			z-index: 10;
		}

		.top_view {
			height: var(--status-bar-height);
			width: 100%;
			color: #000000;
		}

		.geduan {
			// height: calc(var(--status-bar-height) + 80rpx);
		}

		.titles {
			height: 80rpx;
			display: flex;
			align-items: center;
			position: relative;

			.back {
				width: 100rpx;
				height: 80rpx;
				display: flex;
				align-items: center;
				justify-content: center;
				z-index: 10;

				.img {
					width: 21rpx;
					height: 32rpx;
					display: block;
				}
			}

			.titleText {
				position: absolute;
				top: 0;
				left: 0;
				height: 80rpx;
				width: 100%;
				color: #000000;
				display: flex;
				justify-content: center;
				align-items: center;
				font-size: 34rpx;
				font-weight: 600;
			}
		}
	}
</style>
