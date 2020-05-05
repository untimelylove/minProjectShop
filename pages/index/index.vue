<template>
	<view>
		<cu-custom bgColor="bg-jd" :isBack="false">
			<block slot="content">好物优品</block >
		</cu-custom>
		<view class="tui-header" :style="[{top:CustomBar + 'px'}]">
			<view class="tui-category" hover-class="opcity" :hover-stay-time="150" @tap="Support()">
				<tui-icon name="sweep" color="#fff" :size="22" @tap="scan"></tui-icon>
				<view class="tui-category-scale"></view>
			</view>
			<view class="tui-rolling-search">
				<!-- #ifdef APP-PLUS || MP -->
				<icon type="search" :size='13' color='#999'></icon>
				<!-- #endif -->
				<!-- #ifdef H5 -->
				<view>
					<tui-icon name="search" :size='16' color='#999'></tui-icon>
				</view>
				<!-- #endif -->
				<swiper vertical autoplay circular interval="3000" class="tui-swiper">
					<swiper-item v-for="(hot,index) in hotSearch" :key="index" class="tui-swiper-item" @tap="search">
						<view class="tui-hot-item">{{hot}}</view>
					</swiper-item>
				</swiper>
			</view>
		</view>
		<!--选项卡-->
		<view class="zaiui-flex-tab show">
			<view class="flex text-white">
				<view class="basis-xxl">
					<scroll-view scroll-x class="nav z" scroll-with-animation :scroll-left="headTab.scrollLeft">
						<block v-for="(item,index) in headTab.list" :key="index">
							<view class="cu-item" :class="index==headTab.TabCur?'select':''" @tap="tabSelect" :data-id="index">
								<view>{{item.name}}</view>
								<view class="tab-dot bg-white" />
							</view>
						</block>
					</scroll-view>
				</view>
				<view class="basis-xxs">
					<view class="sort-icon" @tap="sortVueTap">
						<text class="cuIcon-sort" />
					</view>
				</view>
			</view>
		</view>
		<swiper v-show="hiddenCon" class="screen-swiper" :class="dotStyle?'square-dot':'round-dot'" :indicator-dots="true"
		 :circular="true" :autoplay="true" interval="5000" duration="500">
			<swiper-item v-for="(item,index) in swiperList" :key="index">
				<image :src="item.picUrl" mode="aspectFill"></image>
				<video :src="item.picUrl" autoplay loop muted :show-play-btn="false" :controls="false" objectFit="cover" v-if="item.type=='video'"></video>
			</swiper-item>
		</swiper>
		<view v-show="!hiddenCon" class="shopItem">
			<view class="shopfo">
				<view class="Side" v-for="(picurl,index) in shopList" :key="index">
					<image :src="picurl.picUrl" mode="aspectFill" class="Icon" />
					<text class="IconTxt">{{picurl.title}}</text>
				</view>
			</view>
		</view>
		<view v-show="hiddenCon" class=" Humzi">
			<view class="kGYGSu"></view>
			<grid-menu-list :list_data='gridMenuData' @listTap='gridMenuTap' />
		</view>
		<!-- 新闻滚动条 -->
		<view class="tui-rolling-news">
			<tui-icon name="news-fill" :size='28' color='#f54f46'></tui-icon>
			<swiper vertical autoplay circular interval="3000" class="tui-swiper">
				<swiper-item v-for="(item,index) in newsList" :key="index" class="tui-swiper-item">
					<view class="tui-news-item" @tap='detail'>{{item}}</view>
				</swiper-item>
			</swiper>
		</view>
		<!-- 新人专享 -->
		<view class="tui-product-box tui-pb-20 tui-bg-white">
			<view class="tui-activity-box" @tap="detail">
				<image src="/static/images/index/activity_1.jpg" class="tui-activity-img" mode="widthFix"></image>
				<image src="/static/images/index/activity_2.jpg" class="tui-activity-img" mode="widthFix"></image>
			</view>
		</view>
		<view class="every-day" v-show="hiddenCon">
			<view class="eve-title">
				<image src="../../static/images/index/everyDat.jpg" mode=""></image>
			</view>
			<view class="bg-white recommend-box">
				<view v-for="(itemUrl,index) in recommendList" :key="index" class="within">
					<view>
						<text class="text-gradient">{{itemUrl.title}}</text>
						<text class="text-sug">{{itemUrl.remark}}</text>
					</view>
					<view class="bg-box">
						<image class="bg-img" :src="itemUrl.picUrl"></image>
					</view>
				</view>
			</view>
		</view>
		<tui-loading :visible="waitting"></tui-loading>
		<view class="recommend">
			<view class="top-img">
				<image src="../../static/images/index/tuijian.png" mode=""></image>
			</view>
			<view class="uni-product-list">
				<view class="uni-product" v-for="(product,index) in productList" :key="index">
					<!-- <skeleton :loading="loading" :avatarSize="skeleton1.avatarSize" :row="skeleton1.row" :showTitle="skeleton1.showTitle"
					 :showAvatar="skeleton1.showAvatar"> -->
					<view class="image-view">
						<image v-if="renderImage" class="uni-product-image" :src="product.image"></image>
					</view>
					<view class="uni-product-title"> <text class="uni-product-tip">{{product.tip}}</text>{{product.title}}</view>
					<view class="uni-product-price">
						<text class="uni-product-price-favour">￥{{product.originalPrice}}</text>
						<text class="uni-product-price-original">￥{{product.favourPrice}}</text>
					</view>
					<!-- </skeleton> -->
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import gridMenuList from '@/components/list/grid-menu-list';
	import tuiIcon from "@/components/icon/icon"
	import Skeleton from "@/components/primewind-skeleton/components/skeleton/index.vue"
	import tuiLoading from "@/components/loading/loading"
	const WXAPI = require('apifm-wxapi')
	export default {
		data() {
			return {
				CustomBar: this.CustomBar,
				StatusBar: this.StatusBar,
				// 划动菜单数据
				gridMenuData: [],
				shopList: [],
				swiperList: [],
				// iconList: [],
				recommendList: [],
				dotStyle: false,
				hiddenCon: true, // 点击Tab隐藏主页新客户和轮播图其他
				// 骨架屏
				loading: true, // 未使用
				hotSearch: [
					"iphone 手机",
					"李宁X故宫",
					"小冰箱迷你"
				],
				newsList: [
					"开发版请勿下单支付商品",
					"开发版请勿下单支付商品",
					"开发版请勿下单支付商品"
				],
				skeleton1: {
					avatarSize: '52px',
					row: 3,
					showTitle: true,
					showAvatar: false,
				},
				title: 'product-list',
				productList: [],
				renderImage: false,
				waitting: false,
				// getRightList:[], // 分类列表
				headTab: {
					TabCur: 0,
					scrollLeft: 0,
					list: []
				},
				categoryHeight: '150px',
			}
		},
		components: {
			tuiIcon,
			Skeleton,
			tuiLoading,
			gridMenuList
		},
		created() {
			this.reloadData()
		},
		onLoad() {
			this.loadData()
			setTimeout(() => {
				this.renderImage = true;
			}, 300);
			WXAPI.goodsCategory({
				level: 1
			}).then(res => {
				if (res.code == 0) {
					const cateLevel1 = res.data.filter(ele => {
						return ele.level == 1
					})
					// const id = cateLevel1[0].id
					// this.getRightTiele(id)
					var indexPage = {
						name: '首页'
					}
					this.headTab.list = cateLevel1
					this.headTab.list.unshift(indexPage)
				}
			})
			// 获取轮播
			WXAPI.banners({
					type: 'banner'
				}).then(res => {
					if (res.code == 0) {
						this.swiperList = res.data
					}
				}),
				WXAPI.banners({
					type: 'icon'
				}).then(res => {
					console.log(res)
					if (res.code == 0) {
						this.gridMenuData = res.data
					}
				}),
				WXAPI.banners({
					type: 'recommend'
				}).then(res => {
					if (res.code == 0) {
						this.recommendList = res.data
					}
				})
				WXAPI.banners({
					type: 'little'
				}).then(res => {
					console.log(res)
					if (res.code == 0) {
						this.shopList = res.data
					}
				})
		},
		methods: {
			//扫一扫
			scan() {
				// uni.scanCode({
				// 	success: (res) => {
				// 		// uni.showToast({
				// 		// 	title: '条码内容：' + res.result
				// 		// });
				// 		console.log(res)
				// 		console.log('条码类型：' + res.scanType);
				// 		console.log('条码内容：' + res.result);
				// 	}
				// });
				uni.navigateTo({
					url:"../shopList/shoplist"
				})
			},
			// 划动菜单跳转
			gridMenuTap(e) {
				console.log(e);
				if (e.index == 7) {
					uni.switchTab({
						url: '../whole/whole'
					});
				} else {
					console.log('跳转商品页')
				}
			},
			loadData(action = 'add') {
				const data = [{
						image: 'https://img-cdn-qiniu.dcloud.net.cn/uploads/example/product1.jpg',
						title: 'Apple iPhone X 256GB 深空灰色 移动联通电信4G手机',
						originalPrice: 9999,
						favourPrice: 8888,
						tip: '自营'
					},
					{
						image: 'https://img-cdn-qiniu.dcloud.net.cn/uploads/example/product2.jpg',
						title: 'Apple iPad 平板电脑 2018年新款9.7英寸',
						originalPrice: 3499,
						favourPrice: 3399,
						tip: '优惠'
					},
					{
						image: 'https://img-cdn-qiniu.dcloud.net.cn/uploads/example/product3.jpg',
						title: 'Apple MacBook Pro 13.3英寸笔记本电脑（2017款Core i5处理器/8GB内存/256GB硬盘 MupxT2CH/A）',
						originalPrice: 12999,
						favourPrice: 10688,
						tip: '秒杀'
					},
					{
						image: 'https://img-cdn-qiniu.dcloud.net.cn/uploads/example/product4.jpg',
						title: 'Kindle Paperwhite电纸书阅读器 电子书墨水屏 6英寸wifi 黑色',
						originalPrice: 999,
						favourPrice: 958,
						tip: '秒杀'
					},
					{
						image: 'https://img-cdn-qiniu.dcloud.net.cn/uploads/example/product5.jpg',
						title: '微软（Microsoft）新Surface Pro 二合一平板电脑笔记本 12.3英寸（i5 8G内存 256G存储）',
						originalPrice: 8888,
						favourPrice: 8288,
						tip: '优惠'
					},
					{
						image: 'https://img-cdn-qiniu.dcloud.net.cn/uploads/example/product6.jpg',
						title: 'Apple Watch Series 3智能手表（GPS款 42毫米 深空灰色铝金属表壳 黑色运动型表带 MQL12CH/A）',
						originalPrice: 2899,
						favourPrice: 2799,
						tip: '自营'
					}
				];

				if (action === 'refresh') {
					this.productList = [];
				}

				data.forEach(item => {
					this.productList.push(item);
				});
			},
			search() {
				uni.navigateTo({
					url: '../search/search'
				})
			},
			// 搜索框下tab菜单
			tabSelect(e) {
				let index = e.currentTarget.dataset.id;
				console.log(index)
				this.headTab.TabCur = index;
				this.headTab.scrollLeft = (index - 1) * 60;
				if (index == 0) {
					this.hiddenCon = true
				} else {
					this.hiddenCon = false
				}
				// uni.pageScrollTo({
				// 	scrollTop: 0,
				// 	duration: 0
				// });
			},
			reloadData() {
				this.loading = true
				setTimeout(() => {
					this.loading = false
				}, 3000)
			},
			sortVueTap() {
				uni.switchTab({
					url: '../whole/whole'
				});
			}
		}
	}
</script>

<style lang="scss" scoped>
	.tui-header {
		width: 100%;
		height: 100rpx;
		padding: 0 20rpx;
		box-sizing: border-box;
		background: #e41f19;
		display: flex;
		align-items: center;
		justify-content: space-between;
		position: fixed;
		left: 0;
		top: 0;
		/* #ifdef H5 */
		top: 44px;
		/* #endif */
		z-index: 999;
	}

	.tui-category {
		font-size: 24rpx;
		line-height: 100rpx;
		color: #fff;
		display: flex;
		align-items: center;
		justify-content: center;
		flex-direction: column;
		margin: 0;
		margin-right: 22rpx;
		flex-shrink: 0;
	}

	.tui-category-scale {
		transform: scale(0.7);
		line-height: 24rpx;
	}

	.tui-rolling-search {
		width: 100%;
		height: 60rpx;
		border-radius: 35rpx;
		padding: 0 40rpx 0 30rpx;
		box-sizing: border-box;
		background: #fff;
		display: flex;
		align-items: center;
		flex-wrap: nowrap;
		color: #999;
	}

	.screen-swiper {
		height: 240rpx;
		// margin-top: 100rpx;
	}

	.Humzi {
		width: 100%;
		position: relative;
		margin-top: -10px;
		z-index: 100;
		color: rgb(102, 102, 102);
	}

	.kGYGSu {
		background-image: url(https://gw.alicdn.com/tfs/TB1vMMwChTpK1RjSZFMXXbG_VXa-1125-30.png);
		height: 10px;
		width: 100%;
		background-size: 100%;
		margin-bottom: -2px;
		background-repeat: no-repeat;
		background-position: center center;
	}

	.shopItem {
		height: 388rpx;
		// white-space: ;
		width: 100%;
	}

	.shopfo {
		display: flex;
		flex-wrap: wrap;
		justify-content: space-between;
		background: rgb(255, 255, 255);
		height: 100%;
		width: 100%;
		padding: 40rpx 20rpx 10rpx;
	}

	.Side {
		width: 20%;
		text-align: center;
	}

	.ALWfo {
		display: flex;
		justify-content: space-around;
		width: 100%;
		height: 100%;
		overflow-x: auto;
		background: rgb(255, 255, 255);
		padding-bottom: 40rpx;
	}

	.Icon {
		width: 86rpx;
		height: 86rpx;
	}

	.IconTxt {
		margin-top: 8rpx;
		display: block;
		text-align: center;
		color: #666666;
		font-size: 22rpx;

	}

	.tui-product-box {
		margin: 20rpx 20rpx 0;
		/* padding: 0 20rpx; */
		box-sizing: border-box;
	}

	/* .tui-pb-20 {
		padding-bottom: 20rpx;
	} */

	.tui-bg-white {
		/* background: #fff; */
	}

	.tui-group-name {
		font-size: 32rpx;
		font-weight: bold;
		text-align: center;
		padding: 24rpx 0;
	}

	.tui-activity-box {
		display: flex;
		border-radius: 12rpx;
		overflow: hidden;
	}

	.tui-activity-img {
		width: 50%;
		display: block;
	}

	.eve-title {
		height: 70rpx;
		text-align: center;
	}

	.every-day {
		box-sizing: border-box;
		margin: 20rpx 20rpx 0;
	}

	.eve-title image {
		height: 100%;
	}

	.text-gradient {
		font-weight: 700;
		background: -webkit-linear-gradient(left, #FF2A2A, #F139D2);
		background: linear-gradient(90deg, #FF2A2A, #F139D2);
		-webkit-background-clip: text;
		color: transparent;
		height: 44rpx;
		line-height: 60rpx;
		font-size: 16px;
		display: inline-block;
	}

	.within:nth-child(2) .text-gradient {
		background: -webkit-linear-gradient(left, #FE8537, #F02B2B);
		background: linear-gradient(90deg, #FE8537, #F02B2B);
		-webkit-background-clip: text;
		color: transparent;
	}

	.within:nth-child(3) .text-gradient {
		background: -webkit-linear-gradient(left, #FF765C, #FF23B3);
		background: linear-gradient(90deg, #FF765C, #FF23B3);
		-webkit-background-clip: text;
		color: transparent;
	}

	.within:nth-child(4) .text-gradient {
		background: -webkit-linear-gradient(left, #FF2A00, #FF00AF);
		background: linear-gradient(90deg, #FF2A00, #FF00AF);
		-webkit-background-clip: text;
		color: transparent;
	}

	.within:nth-child(5) .text-gradient {
		background: -webkit-linear-gradient(left, #D400FF, #FF320A);
		background: linear-gradient(90deg, #D400FF, #FF320A);
		-webkit-background-clip: text;
		color: transparent;
	}

	.within:nth-child(6) .text-gradient {
		background: -webkit-linear-gradient(left, #00C2AB, #3E94FF);
		background: linear-gradient(90deg, #00C2AB, #3E94FF);
		-webkit-background-clip: text;
		color: transparent;
	}

	.within:nth-child(7) .text-gradient {
		background: -webkit-linear-gradient(left, #FF765C, #FF23B3);
		background: linear-gradient(90deg, #FF765C, #FF23B3);
		-webkit-background-clip: text;
		color: transparent;
	}

	.within:nth-child(8) .text-gradient {
		background: -webkit-linear-gradient(left, #2AD396, #85BB1F);
		background: linear-gradient(90deg, #2AD396, #85BB1F);
		-webkit-background-clip: text;
		color: transparent;
	}

	.recommend-box {
		width: 100%;
		height: 100%;
		border-radius: 16rpx;
	}

	.within {
		width: 25%;
		height: 100%;
		display: inline-block;
		text-align: center;
		padding: 12rpx 0 10rpx 0;
		border-right: 1px solid #F5F5F5;
	}

	.within:nth-child(1),
	.within:nth-child(2),
	.within:nth-child(3),
	.within:nth-child(4) {
		border-bottom: 1px solid #F5F5F5;
	}

	.within:nth-child(4),
	.within:nth-child(8) {
		border-right: 0;
	}

	.bg-box {
		height: 120rpx;
		width: 120rpx;
		display: inline-block;
	}

	.bg-img {
		width: 100%;
		height: 100%;
	}

	.text-sug {
		font-size: 12px;
		white-space: nowrap;
		text-overflow: ellipsis;
		overflow: hidden;
		width: 100%;
		display: inline-block;
		line-height: 60rpx;
	}

	.recommend {
		box-sizing: border-box;
		margin: 20rpx 20rpx 0;
		height: 100%;
	}

	.top-img {
		height: 70rpx;
		text-align: center;
	}

	.top-img image {
		height: 100%;
	}

	.uni-product-list {
		display: flex;
		width: 100%;
		flex-wrap: wrap;
		flex-direction: row;
		justify-content: space-between;
	}

	.uni-product {
		flex-direction: column;
		background: #ffffff;
		width: 49%;
		border-radius: 18rpx;
		margin-bottom: 22rpx;
	}

	.image-view {
		height: 330upx;
		width: 330upx;
		margin: 12upx auto;
	}

	.uni-product-image {
		height: 330upx;
		width: 330upx;
	}

	.uni-product-title {
		width: 300upx;
		/* 在恰当的断字点进行换行： */
		word-break: break-all;
		display: -webkit-box;
		overflow: hidden;
		line-height: 1.5;
		text-overflow: ellipsis;
		-webkit-box-orient: vertical;
		/* 最多展示两行 */
		-webkit-line-clamp: 2;
		margin: 0 auto;
	}

	.uni-product-price {
		margin: 10upx auto;
		font-size: 28upx;
		line-height: 1.5;
		position: relative;
	}

	.uni-product-price-original {
		color: #e80080;
	}

	.uni-product-price-favour {
		color: #888888;
		text-decoration: line-through;
		margin-left: 10upx;
	}

	.uni-product-tip {
		background-color: #ff3333;
		margin-right: 10upx;
		color: #ffffff;
		padding: 0 10upx;
		border-radius: 6upx;
	}

	.tui-swiper {
		font-size: 26rpx;
		height: 60rpx;
		flex: 1;
		padding-left: 12rpx;
	}

	.tui-swiper-item {
		display: flex;
		align-items: center;
	}

	.tui-hot-item {
		line-height: 26rpx;
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
	}

	.tui-rolling-news {
		/* width: 100%; */
		border-radius: 8rpx;
		padding: 12upx 30upx;
		box-sizing: border-box;
		display: flex;
		align-items: center;
		justify-content: center;
		flex-wrap: nowrap;
		background: #FFFFFF;
		margin: 20rpx 20rpx 0;
	}

	.tui-swiper {
		font-size: 28upx;
		height: 50upx;
		flex: 1;
	}

	.tui-swiper-item {
		display: flex;
		align-items: center
	}

	.tui-news-item {
		line-height: 28upx;
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
	}

	.zaiui-flex-tab {
		// position: relative;
		transition: opacity .25s;
		// top: 100rpx;
		margin-top: 100rpx;
		background: #e41f19;


		.flex {
			.basis-xxl {
				flex-basis: 90%;
				width: 90%;
				z-index: 1;
			}

			.basis-xxs {
				flex-basis: 10%;
				z-index: 1;
				width: 10%;
				display: flex;
				align-items: center;
			}

			.sort-icon {
				font-size: 55rpx;
				height: 64rpx;
				line-height: 64rpx;
				// text-align: center;
				margin: 0 auto;
			}
		}
	}

	.select {
		font-size: 30rpx;
		font-weight: bold;
	}
</style>
