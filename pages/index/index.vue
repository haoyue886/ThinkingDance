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
			<audio id="myAudio" :src="audioUrl" autoplay loop></audio>
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
					url: 'https://pcsdata.baidu.com/thumbnail/e69230404g9983290a2c9891f4fdc83a?fid=3159939267-16051585-179369120847406&rt=pr&sign=FDTAER-yUdy3dSFZ0SVxtzShv1zcMqd-cJNByhVW4s5%2Bx5yxQhY4rWuhfOs%3D&expires=2h&chkv=0&chkbd=0&chkpc=&dp-logid=2427659356&dp-callid=0&time=1622016000&size=c1600_u1600&quality=100&vuk=-&ft=video',
					id: 1
				}, {
					url: 'https://pcsdata.baidu.com/thumbnail/f7a624803k0ef1627ed5e980cb36172c?fid=3159939267-16051585-101544136594303&rt=pr&sign=FDTAER-yUdy3dSFZ0SVxtzShv1zcMqd-6T10O1VNmtyb4ZU8AkKy3%2FNAAi8%3D&expires=2h&chkv=0&chkbd=0&chkpc=&dp-logid=2637449066&dp-callid=0&time=1622019600&size=c1600_u1600&quality=100&vuk=-&ft=video',
					id: 2
				}, {
					url: 'https://pcsdata.baidu.com/thumbnail/ccbd3d9f5l4edc0cf7c3482cf9daba82?fid=3159939267-16051585-1038846611864731&rt=pr&sign=FDTAER-yUdy3dSFZ0SVxtzShv1zcMqd-C58bswc4uV93wM4v5aOH5YIB%2Fuw%3D&expires=2h&chkv=0&chkbd=0&chkpc=&dp-logid=2664185888&dp-callid=0&time=1622019600&size=c1600_u1600&quality=100&vuk=-&ft=video',
					id: 3
				}, {
					url: 'https://pcsdata.baidu.com/thumbnail/0b6a2ee1cv7aec478e119f615a9b65d4?fid=3159939267-16051585-995592652578382&rt=pr&sign=FDTAER-yUdy3dSFZ0SVxtzShv1zcMqd-TH7Jr1o7oJiSibX3uL%2B%2FKgGr7K8%3D&expires=2h&chkv=0&chkbd=0&chkpc=&dp-logid=2609422452&dp-callid=0&time=1622019600&size=c1600_u1600&quality=100&vuk=-&ft=video',
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
			const that = this
			that.isPlay = true
			//that.getMusicUrl()
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
</style>
