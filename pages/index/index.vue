<template>

	<view class="content">

		<!-- 轮播图 -->
		<carousel :img-list="imgList" url-key="url" @selected="selectedBanner" />
		<!-- 搜索框 -->
		<view id="toSearch">
			<input type="text" placeholder="输入关键字查询..." confirm-type="search">
			<image src="../../static/images/search.png" id="search_img"></image>
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
			<audio id="myAudio" ></audio>
			<!-- <audio id="myAudio" :src="audioUrl" autoplay loop></audio> -->
			<!-- 导航+日历	 -->
			<view class="nav_box1" @tap="gotoTimeline">
				<view class="co-center cuIcon-time"></view>
				<text class="ch-font">时间轴</text>
			</view>
			<view class="nav_box2 "@tap="gotoNotes">
				<view class="co-center cuIcon-creative"></view>
				<text class="ch-font">捕捉灵感</text>
			</view>
				<view class="mask">
					<view class="nav_box3 ">
						<view id="t_date">
							<view>{{date_m}}月{{date_d}}日</view>
						</view>
						<view id="t_day"><span>{{date_day}}</span></view>
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
					url: 'https://wx2.sinaimg.cn/mw690/006ghMQ6gy1gqx8k7323xj30b507o40v.jpg',
					id: 1
				}, {
					url: 'https://wx3.sinaimg.cn/mw690/006ghMQ6gy1gqx8k71vgzj30kb0bggn9.jpg',
					id: 2
				}, {
					url: 'https://wx3.sinaimg.cn/mw690/006ghMQ6gy1gqx8k72572j30kt0dxmyf.jpg',
					id: 3
				}, {
					url: 'https://wx1.sinaimg.cn/mw690/006ghMQ6gy1gqx8k742kgj30rm0fkn08.jpg',
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
			this.getTime(this);
			const that = this
			that.audioCtx = uni.createAudioContext('myAudio')
			const innerAudioContext = uni.createInnerAudioContext();
			innerAudioContext.autoplay = true;
			innerAudioContext.src = 'https://win-web-rb01-sycdn.kuwo.cn/979f33d22e95aacd336dfc7ee34eb8a6/60af261a/resource/n1/21/38/3371198176.mp3';
			innerAudioContext.onPlay(() => {
    //            const that = this
				// if (that.isPlay) {
				// 	that.audioCtx.pause()
				// 	that.isPlay = !that.isPlay
				// 	tools.showToast('您已暂停音乐播放~')
				// } else {
				// 	that.audioCtx.play()
				// 	that.isPlay = !that.isPlay
				// 	tools.showToast('背景音乐已开启~')
				// }
			});
		},
		onShow: function() {
			const that = this
			that.isPlay = true
			//that.getMusicUrl()
			let innerAudioContext = uni.createInnerAudioContext()
			
		},

		methods: {
			// 导航跳转
			gotoTimeline() {
				uni.navigateTo({
					url: '../timeline/timeline'
				})
			},
				// 导航跳转
						gotoNotes(){
							uni.navigateTo({
								url:'../notes/notes'
							})
						},
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
			}
			// 从云数据库获取音乐
			// getMusicUrl: function() {
			// 	const that = this
			// 	const db = uni.cloud.database()
			// 	const music = db.collection('music')
			// 	music.get().then(res => {
			// 		that.audioUrl = res.data[0].musicUrl
			// 		that.audioCtx.loop = true
			// 		that.audioCtx.play()
			// 	})
			// }
			
			
			
		}
	}
</script>
<style>
	@import url("../../static/style/index.css");
</style>
