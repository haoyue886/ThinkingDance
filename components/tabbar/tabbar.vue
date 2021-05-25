<template>
	<view class="tabbar-main">
		<view v-for="(item, index) in tabbarList" :key="index" class="tabbar-item" :data-path="item.pagePath"
			@tap="changePage" :data-index="index">
			<view class="tabbar_icon flex_c">
				<view v-if="nowIndex==index?'true':''" class="showb">
					<image :src="item.onIcon" class="icon"></image>
				</view>
				<view v-else>
					<image :src="item.icon" class="icon"></image>
				</view>
			</view>
			<!-- tabbar文字 -->
			<!-- <view class="tabbar_title" v-if="item.index !== 1">
				<text :style="'color:' + ( nowIndex === index ? item.onColor : item.color )">{{ item.title }}</text>
			</view> -->
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tabbarList: [{
						index: 0,
						pagePath: '../painting/painting',
						title: "画板",
						icon: "/static/images/huaban.png",
						onIcon: "/static/images/huaban.png",
						color: "#9B9B9B",
						onColor: "#19C29C"
					},
					{
						index: 1,
						pagePath: '../index/index',
						title: "首页",
						icon: "/static/images/house.png",
						onIcon: "/static/images/house.png",
						color: "#9B9B9B",
						onColor: "#19C29C"
					},
					{
						index: 2,
						pagePath: '../user/user',
						title: "个人中心",
						icon: "/static/images/user.png",
						onIcon: "/static/images/user.png",
						color: "#9B9B9B",
						onColor: "#19C29C"
					}
				]
			};
		},

		components: {},
		props: {
			nowIndex: String
		},
		options: {
			// 共享全局样式
			addGlobalClass: true
		},
		methods: {
			// 切换
			changePage(e) {
				let {
					path,
					index
				} = e.currentTarget.dataset;

				uni.switchTab({
					url: path
				});
			}
		}
	};
</script>
<style>
	/* tabbar样式 */
	.tabbar-main {
		position: fixed;
		display: flex;
		align-items: stretch;
		z-index: 1;
		width: calc(100vw - 80rpx);
		margin-left: 40rpx;
		border-radius: 70rpx;
		height: 98rpx;
		left: 0;
		bottom: 20rpx;
		background-color:#5B4CB1;
		/*背景色 */
		/* border-top: 2rpx solid #F1F1F1; */
	}

	.tabbar-item {
		flex: 1;
	}

	.tabbar_icon {
		width: 100%;
		height: 100%;

	}

	.flex_c {
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.icon {
		width: 50rpx;
		height: 48rpx;
	}

	.tabbar_title {
		font-size: 24rpx;
		width: 100%;
		height: 38rpx;
	}

	/* 选中背景 */
.showb{
	position: absolute;
	top:-5rpx;
	box-shadow: 2rpx 0rpx 10rpx #F4F8F9;
  background-color: #F4F8F9;
  width: 88rpx;
  height: 92rpx;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 50%;
}

</style>
