<script>
	export default {
<<<<<<< HEAD
		onLaunch: function() {
			console.log('App Launch')
		},
		onShow: function() {
=======
		onLaunch: function() { 
			wx.cloud.init({
				env: 'cloud1-8g5ro01u8b306dc8',
				traceUser: true
			})

			console.log('App Launch');
		},
		onShow: function() {
			// 隐藏原生tabbar
			uni.hideTabBar()
>>>>>>> 7d72864 (ui设计)
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	/*每个页面公共css */
	@import url("./colorui/icon.css");
	@import url("./colorui/main.css");
<<<<<<< HEAD
	@import './static/iconfont-weapp-icon.css';
=======
	@import url("./colorui/animation.css");
	@import url("static/style/iconfont.css");
>>>>>>> 7d72864 (ui设计)
</style>
