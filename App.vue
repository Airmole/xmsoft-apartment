<script>
	import Vue from 'vue'
	export default {
		onLaunch: function() {
			
			// #ifdef APP-PLUS
			//app关闭默认的启动 方法关闭启动图。但是这个时间不能太晚，6s 超时后依旧会主动关闭。
			setTimeout(()=>{
				plus.navigator.closeSplashscreen();
			},100)
			// #endif
			
			uni.getSystemInfo({
			success: function(e) {
				  // #ifndef MP
				  Vue.prototype.StatusBar = e.statusBarHeight;
				  if (e.platform == 'android') {
					Vue.prototype.CustomBar = e.statusBarHeight + 50;
				  } else {
					Vue.prototype.CustomBar = e.statusBarHeight + 45;
				  };
				  // #endif
				  // #ifdef MP-WEIXIN
				  Vue.prototype.StatusBar = e.statusBarHeight;
				  let custom = wx.getMenuButtonBoundingClientRect();
				  Vue.prototype.Custom = custom;
				  Vue.prototype.CustomBar = custom.bottom + custom.top - e.statusBarHeight;
				  // #endif		
				  // #ifdef MP-ALIPAY
				  Vue.prototype.StatusBar = e.statusBarHeight;
				  Vue.prototype.CustomBar = e.statusBarHeight + e.titleBarHeight;
				  // #endif
				}
			})
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	/*每个页面公共css */
	@import "colorui/main.css";
	@import "colorui/icon.css";
</style>
