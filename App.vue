<style>
	@import "colorui/main.css";
	@import "colorui/icon.css";
	@import "common/icon/iconfont.css";
	@import "./common/app.css";
</style>
<script>
	import config from './api/config.js'
	import WXAPI from 'apifm-wxapi'
	import Vue from 'vue'
	export default {
		onLaunch: function() {
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
			console.log('App Launch')
			WXAPI.init(config.subDomain) // 从根目录的 config.js 文件中读取
			const that = this;
			//  获取接口和后台权限
			WXAPI.vipLevel().then(res => {
				that.globalData.vipLevel = res.data
				console.log(that.globalData.vipLevel)
			})
			//  获取商城名称
			WXAPI.queryConfigBatch(
				'mallName,recharge_amount_min,WITHDRAW_MIN,ALLOW_SELF_COLLECTION,RECHARGE_OPEN,categoryLevel,CATEGORY_BY_TAGS').then(
				function(res) {
					if (res.code == 0) {
						res.data.forEach(config => {
							wx.setStorageSync(config.key, config.value);
							if (config.key === 'recharge_amount_min') {
								that.globalData.recharge_amount_min = config.value;
								console.log(that.globalData.recharge_amount_min)
							}
						})

					}
				})
		},
		globalData: {
			isConnected: true,
			launchOption: undefined,
			vipLevel: 0
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
</style>
