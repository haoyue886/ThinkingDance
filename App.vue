<script>
	export default {
		onLaunch: function() {
			
			wx.cloud.init({
				env:'cloud1-8g5ro01u8b306dc8', 
				traceUser:true
			})

			console.log('App Launch');
		},
		onShow: function() {
			// 隐藏原生tabbar
			uni.hideTabBar()
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
	@import url("static/style/iconfont.css");
</style>
