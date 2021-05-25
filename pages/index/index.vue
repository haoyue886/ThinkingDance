<template>
<<<<<<< HEAD
	<view class="content">
	<text>内容</text>
	</view>

</template>

<script>
	export default {
		data() {
			return {
				title: 'Hello'
			}
		},
		onLoad() {

		},
		methods: {

		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
=======

	<view class="content">

		<!-- 轮播图 -->
		<carousel :img-list="imgList" url-key="url" @selected="selectedBanner" />
		<!-- 搜索框 -->
		<view id="toSearch">
			<input type="text" placeholder="输入关键字查询..." confirm-type="search">
			<image src="../../static/icon/search.png" id="search_img"></image>
		</view>
		<!-- 内容 -->
		<view class="nav">
			<!-- 背景圆点 -->
			<view class="small_blue"></view>
			<view class="yellow_circular"></view>
			<view class="blue_circular"></view>
			<!-- 音乐模块 -->
			<view class="bg_music" @tap="audioPlay">
				<image src="../../static/images/music_icon.png" class="musicImg" :class="isPlay?'music_icon':''" />
				<image src="../../static/images/music_play.png" class="music_play"
					:class="isPlay?'pauseImg':'playImg'" />
			</view>
			<audio id="myAudio" :src="audioUrl" autoplay loop></audio>
			<!-- 导航+日历	 -->
			<view class="nav_box1" @tap="gotoTimeline">
				<view class="co-center cuIcon-time"></view>
				<text class="ch-font">时间轴</text>
			</view>
			<view class="nav_box2 timeline">
				<view class="co-center cuIcon-creative"></view>
				<text class="ch-font">捕捉灵感</text>
			</view>
				<view class="mask">
					<view class="nav_box3 ">
						<view id="t_date">
							<view>{{date_m}} / {{date_d}}</view>
							<view id="date_yy">{{date_y}}</view>
						</view>
						<view id="t_day"><span>{{date_day}}</span></view>
						<text>[您的思维累积活跃{{pass}}次]</text>
				
					</view>
				</view>

		</view>
		<!-- 自定义tabbar -->
		<view  v-if="show" @tap="switchDialog">	</view>
		<Tabbar nowIndex="1" @switchDialog="switchDialog"></Tabbar>
	</view>
</template>



<script>
	import Tabbar from "../../components/tabbar/tabbar";
	import carousel from '@/components/vear-carousel/vear-carousel';
	
	export default {
		components: {
			carousel,
			Tabbar
		},
		data: function() {
			return {
				// 自定义tabbar页面展示
				show: false,
				// 轮播图
				imgList: [{
					url: 'https://img9.51tietu.net/pic/2019-091200/vgkpidei2tjvgkpidei2tj.jpg',
					id: 1
				}, {
					url: 'https://img9.51tietu.net/pic/2019-091200/euzekmi5m23euzekmi5m23.jpg',
					id: 2
				}, {
					url: 'https://img9.51tietu.net/pic/2019-091200/143tt0ta4sr143tt0ta4sr.jpg',
					id: 3
				}, {
					url: 'https://img9.51tietu.net/pic/2019-091200/ff1vqwm3q33ff1vqwm3q33.jpg',
					id: 4
				}, ],
				'pass': '7',
				'date_d': '00',
				'date_y': '2020',
				'date_m': 'Aay',
				'date_day': '星期一',
				isPlay: true,
				audioCtx: ''
			}
		},
		onLoad: function() {
			this.gotoTimeline();
			this.getTime(this);
			const that = this
			that.audioCtx = uni.createAudioContext('myAudio')
			//that.getMusicUrl()
		},
		onShow: function() {
			// console.log('11111',calendar)
			const that = this
			that.isPlay = true
			//that.getMusicUrl()
		},

		methods: {
			// 自定义页面
			switchDialog() {
				let bool = this.show;
				this.setData({
					show: !bool
				});
			},
			// 轮播图
			selectedBanner(item, index) {
				console.log('🥒', item, index)
			},
			feedback() {
				uni.navigateTo({
					url: '/pages/user/feedback'
				})
			},
			// 导航跳转
			gotoTimeline() {
				uni.navigateTo({
					url: '../timeline/timeline'
				})
			},
			//获取当前时间
			getTime() {
				let date = new Date();

				var y = date.getFullYear();

				var m = date.getMonth();
				m = m + 1;

				let d = date.getDate();
				d = d < 10 ? ('0' + d) : d; //天补0

				let dd = date.getDay();
				switch (dd) {
					case 0:
						dd = '星期一';
						break;
					case 1:
						dd = '星期二';
						break;
					case 2:
						dd = '星期三';
						break;
					case 3:
						dd = '星期四';
						break;
					case 4:
						dd = '星期五';
						break;
					case 5:
						dd = '星期六';
						break;
					case 6:
						dd = '星期日';
						break;
				}
				this.date_d = d;
				this.date_y = y;
				this.date_m = m;
				this.date_day = dd;
			},
			audioPlay: function() {
				const that = this
				if (that.isPlay) {
					that.audioCtx.pause()
					that.isPlay = !that.isPlay
					tools.showToast('您已暂停音乐播放~')
				} else {
					that.audioCtx.play()
					that.isPlay = !that.isPlay
					tools.showToast('背景音乐已开启~')
				}
			},
			// 从云数据库获取音乐
			/*getMusicUrl: function() {
				const that = this
				const db = uni.cloud.database()
				const music = db.collection('music')
				music.get().then(res => {
					that.audioUrl = res.data[0].musicUrl
					that.audioCtx.loop = true
					that.audioCtx.play()
				})
			},*/
		}
	}
</script>
<style>
	@import url("../../static/style/index.css");
>>>>>>> 7d72864 (ui设计)
</style>
