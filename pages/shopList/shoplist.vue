<template>
	<view class="container">
		<cu-custom bgColor="bg-jd" :isBack="true">
			<block slot="backText"></block>
			<block slot="content">搜索结果</block>
		</cu-custom>
		<!--header-->
		<view class="tui-header-box" :style="[{top:CustomBar + 'px'}]">
			<view class="tui-header" :style="{width:width+'%',height:height+'px'}">
				<!-- #ifndef MP-WEIXIN -->
				<view class="tui-back" @tap="back">
					<tui-icon name="arrowleft" color="#000"></tui-icon>
				</view>
				<!-- #endif -->
				<view class="tui-searchbox tui-search-mr" @tap="search">
					<!-- #ifdef APP-PLUS || MP -->
					<icon type="search" :size='13' color='#999'></icon>
					<!-- #endif -->
					<text class="tui-search-text" v-if="!searchKey">搜索Thorui商品</text>
					<view class="tui-search-key" v-if="searchKey">
						<view class="tui-key-text">{{searchKey}}</view>
						<tui-icon name="shut" :size='12' color='#fff'></tui-icon>
					</view>
				</view>
			</view>
		</view>
		<!--header-->
		<!-- 下拉菜单 -->
		<qit-list :dataList="dataList"></qit-list>
	</view>
</template>

<script>
	const WXAPI = require('apifm-wxapi')
	import tuiIcon from "@/components/icon/icon"
	import QitList from '@/components/Qit-list/Qit-list.vue'
	export default {
		components: {
			tuiIcon,
			QitList
			// tuiDrawer,
			// tuiLoadmore,
			// tuiNomore,
			// tuiTopDropdown
		},
		data() {
			return {
				CustomBar: this.CustomBar,
				StatusBar: this.StatusBar,
				searchKey: '', //搜索关键词
				// width: 200, //header宽度
				width: 100,
				height: 44, //header高度
				// inputTop: 0, //搜索框距离顶部距离
				// arrowTop: 0, //箭头距离顶部距离
				isList: false, //是否以列表展示  | 列表或大图
				dataList: []
			}
		},
		onLoad(options) {
			let obj = {};
			// #ifdef MP-WEIXIN
			obj = wx.getMenuButtonBoundingClientRect();
			// #endif
			// #ifdef MP-BAIDU
			obj = swan.getMenuButtonBoundingClientRect();
			// #endif
			// #ifdef MP-ALIPAY
			my.hideAddToDesktopMenu();
			// #endif
			uni.getSystemInfo({
				success: (res) => {
					// this.width = obj.left || res.windowWidth;
					// this.height = obj.top ? (obj.top + obj.height + 8) : (res.statusBarHeight + 44);
					// this.inputTop = obj.top ? (obj.top + (obj.height - 30) / 2) : (res.statusBarHeight + 7);
					// this.arrowTop = obj.top ? (obj.top + (obj.height - 32) / 2) : (res.statusBarHeight + 6);
					this.searchKey = options.searchKey || "";
					this.getShopList()
					//略小，避免误差带来的影响
					// this.dropScreenH = this.height * 750 / res.windowWidth + 186;
					// this.drawerH = res.windowHeight - uni.upx2px(100) - this.height
				}
			})
		},
		methods: {
			getShopList() {
				WXAPI.goods({
					nameLike: this.searchKey
				}).then(res => {
					if (res.code == 0) {
						this.dataList = res.data
					}
				})
			},
			search() {
				uni.navigateBack({
					delta: 1
				})
			}
		}
	}
</script>

<style>
	page {
		background: #f7f7f7;
	}

	.container {
		padding-bottom: env(safe-area-inset-bottom);
	}

	/* header */
	.tui-header-box {
		width: 100%;
		height: 88rpx;
		background: #fff;
		position: fixed;
		z-index: 99998;
		left: 0;
		top: 0;
	}

	.tui-header {
		display: flex;
		align-items: flex-start;
	}

	.tui-back {
		margin-left: 8rpx;
		height: 32px !important;
		width: 32px !important;
		margin-top: 14rpx;
	}

	.tui-searchbox {
		width: 100%;
		height: 30px;
		margin-right: 30rpx;
		margin-top: 14rpx;
		border-radius: 15px;
		font-size: 12px;
		background: #f7f7f7;
		padding: 3px 10px;
		box-sizing: border-box;
		color: #999;
		display: flex;
		align-items: center;
		overflow: hidden;
	}

	/* #ifdef MP-WEIXIN */
	.tui-search-mr {
		margin-right: 20rpx !important;
		margin-left: 20rpx !important;
	}

	/* #endif */
	/* #ifdef MP-BAIDU */
	.tui-search-mr {
		margin-right: 20rpx !important;
	}

	/* #endif */
	.tui-search-text {
		padding-left: 16rpx;
	}

	.tui-search-key {
		max-width: 80%;
		height: 100%;
		padding: 0 16rpx;
		margin-left: 12rpx;
		display: flex;
		align-items: center;
		/* border-radius: 15px; */
		/* background: rgba(0, 0, 0, 0.5); */
		color: #000;
	}

	.tui-key-text {
		box-sizing: border-box;
		padding-right: 12rpx;
		font-size: 12px;
		line-height: 12px;
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
	}
</style>
