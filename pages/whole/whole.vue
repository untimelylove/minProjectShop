<template>
	<view class="container">
		<view class="tui-searchbox">
			<view class="tui-search-input" @tap="search">
				<!-- #ifdef APP-PLUS || MP -->
				<icon type="search" size='13' color='#999'></icon>
				<!-- #endif -->
				<text class="tui-search-text">搜索好物优品</text>
			</view>
		</view>
		<scroll-view scroll-y scroll-with-animation class="tab-view" :scroll-top="scrollTop" :style="{height:height+'px',top:top+'px'}">
			<view v-for="(item,index) in tabbarList" :key="index" class="tab-bar-item" :class="[currentTab==index ? 'active' : '']"
			 :data-current="index" @tap.stop="swichNav(index,item.id)">
				<text>{{item.name}}</text>
			</view>
		</scroll-view>

		<view v-for="(title,index) in shoptitle" :key="index">
			<scroll-view scroll-y class="right-box" :style="{height:height+'px',top:top+'px'}" v-if="currentTab==index">
				<view class="page-title">{{title[0]}}</view>
				<!-- <view class="page-title">手机</view> -->
				<view class="page-view">
					<view class="page-item" @tap.stop="productList" :data-key="shop1.name" v-for="(shop,index) in shopList1" :key="index">
						<image :src="shop.icon" class="page-image" />
						<view class="g-title">{{shop.name}}</view>
					</view>
				</view>
				<view class="page-title">{{title[1]}}</view>
				<!-- <view class="page-title">手机配件</view> -->
				<view class="page-view">
					<view class="page-item" @tap.stop="productList" :data-key="shop.name" v-for="(shop,index) in shopList2" :key="index">
						<image :src="shop.icon" class="page-image" />
						<view class="g-title">{{shop.name}}</view>
					</view>
				</view>
				<view class="page-title">{{title[2]}}</view>
				<!-- </view><view class="page-title">运营商</view> -->
				<view class="page-view">
					<view class="page-item" @tap.stop="productList" :data-key="shop.name" v-for="(shop,index) in shopList3" :key="index">
						<image :src="shop.icon" class="page-image" />
						<view class="g-title">{{shop.name}}</view>
					</view>
				</view>
				<view class="arrive-down">
					<view class="left"></view>
					<text class="bottom-text">到底啦</text>
					<view class="right"></view>
				</view>
			</scroll-view>
		</view>
		<tui-loading :visible="waitting"></tui-loading>
	</view>
</template>
<script>
	import tuiLoading from "@/components/loading/loading"
	const WXAPI = require('apifm-wxapi')
	export default {
		data() {
			return {
				height: 0, //scroll-view高度
				top: 0,
				currentTab: 0, //预设当前项的值
				scrollTop: 0, //tab标题的滚动条位置
				tabbarList: [],
				// titleList: [],
				shoptitle: [
					['手机', '手机配件', '运营商'],
					['上装', '下装', '外套'],
					['女士包袋', '工具类箱包', '男士包袋'],
					['裤子', '上装', '正装'],
					['时尚', '美妆', '食品'],
					['户外运动', '健身塑形', '小众运动'],
					['影音娱乐', '数码配件', '电脑笔记本'],
					['厨房小电器', '个护电器', '生活电器'],
					['潮搭', '手表', '眼镜'],
					['女鞋', '男鞋', '流行趋势'],
					['袜子', '家居', '内裤'],
					['护肤', '彩妆', '美容美体']
				],
				shopList1: [],
				shopList2: [],
				shopList3: [],
				level: 1, //默认分类级别
				waitting: false
			}
		},
		components: {
			tuiLoading
		},
		onLoad() {
			setTimeout(() => {
				uni.getSystemInfo({
					success: (res) => {
						let header = 92;
						let top = 0;
						//#ifdef H5
						top = 44;
						//#endif
						// res.windowHeight 当前屏幕可用高度
						this.height = res.windowHeight - uni.upx2px(header)
						this.top = top + uni.upx2px(header)
						// console.log( res.windowHeight)
						// console.log(this.height,this.top)
					}
				});
			}, 50)
			// 获取左侧所有分类 按照上级的id找对应下级的pid
			WXAPI.goodsCategory({
				level: this.level
			}).then(res => {
				if (res.code == 0) {
					const cateLevel1 = res.data.filter(ele => {
						return ele.level == 1
					})
					const id = cateLevel1[0].id
					this.getRightTiele(id)
					this.tabbarList = cateLevel1
				}
			})
		},
		methods: {
			// 右侧分类标题
			getRightTiele(id) {
				this.waitting = true
				WXAPI.goodsCategory({
					pid: id
				}).then(res => {
					console.log(res)
					if (res.code == 0) {
						const cateLevel2 = res.data.filter(ele => {
							return ele.pid == id
						})
						this.titleList = cateLevel2
						for (let item of this.titleList) {
							let id = item.id
							console.log(id)
							this.getRightShop(id)
						}
						console.log(this.titleList)
					}
				})
			},
			// 右侧分类商品
			getRightShop(id) {
				WXAPI.goodsCategory({
					pid: id
				}).then(res => {
					if (res.code == 0) {
						const cateLevel3 = res.data.filter(ele => {
							return ele.pid == id
						})
						if (id == 118841 || id == 118978 || id == 119033 || id == 119060 || id == 119090 || id == 119231 || id ==
							119245 || id == 119258 || id == 119274 || id == 119297 || id == 119300 || id == 119313) {
							this.shopList1 = cateLevel3
						} else if (id == 118842 || id == 118979 || id == 119034 || id == 119061 || id == 119091 || id == 119232 || id ==
							119246 || id == 119259 || id == 119285 || id == 119298 || id == 119311 || id == 119314) {
							this.shopList2 = cateLevel3
						} else {
							this.shopList3 = cateLevel3
						}
					}
					this.waitting = false
				})
			},
			// 点击标题切换当前页时改变样式
			swichNav: function(index, id) {
				console.log(index)
				console.log(id)
				if (this.currentTab == index) {
					return false;
				} else {
					this.currentTab = index;
					this.getRightTiele(id)
					this.checkCor();
				}
			},
			//判断当前滚动超过一屏时，设置tab标题滚动条 达到滚动条滚动。
			checkCor() {
				let that = this;
				//这里计算按照实际情况进行修改，动态数据要进行动态分析
				//思路：窗体高度/单个分类高度 200rpx 转px计算 =>得到一屏幕所显示的个数，结合后台传回分类总数进行计算
				//数据很多可以多次if判断然后进行滚动距离计算即可
				if (that.currentTab > 7) {
					that.scrollTop = 500
				} else {
					that.scrollTop = 0
				}
			},
		}
	}
</script>
<style>
	.tui-searchbox {
		width: 100%;
		height: 92upx;
		padding: 0 30upx;
		box-sizing: border-box;
		background: #fff;
		display: flex;
		align-items: center;
		justify-content: center;
		position: fixed;
		left: 0;
		top: 0;
		/* #ifdef H5 */
		top: 44px;
		/* #endif */
		z-index: 100;
	}

	.tui-searchbox::after {
		content: '';
		position: absolute;
		border-bottom: 1upx solid #d2d2d2;
		-webkit-transform: scaleY(0.5);
		transform: scaleY(0.5);
		bottom: 0;
		right: 0;
		left: 0;
	}

	.tui-search-input {
		width: 100%;
		height: 60upx;
		background: #f1f1f1;
		border-radius: 30upx;
		font-size: 26upx;
		color: #999;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.tui-search-text {
		padding-left: 16upx;
	}

	.tab-view {
		/* height: 100%; */
		width: 200upx;
		position: fixed;
		left: 0;
		z-index: 10;
	}

	.tab-bar-item {
		width: 200upx;
		height: 110upx;
		background: #f6f6f6;
		box-sizing: border-box;
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 26upx;
		color: #444;
		font-weight: 400;
	}

	.active {
		position: relative;
		color: #000;
		font-size: 30upx;
		font-weight: 600;
		background: #fcfcfc;
	}

	.active::before {
		content: "";
		position: absolute;
		border-left: 8upx solid #E41F19;
		height: 30upx;
		left: 0;
	}

	/* 右侧 */
	.right-box {
		width: 100%;
		position: fixed;
		padding-left: 226upx;
		padding-right: 26upx;
		box-sizing: border-box;
		left: 0;
	}

	.page-view {
		width: 100%;
		overflow: hidden;
		/* 	padding-top: 20upx;
		padding-right: 20upx; */
		box-sizing: border-box;
		padding-bottom: env(safe-area-inset-bottom);
		display: flex;
		display: -webkit-flex;
		justify-content: flex-start;
		flex-direction: row;
		flex-wrap: wrap;
		background-color: #FFFFFF;
		border-radius: 30rpx;
		padding-bottom: 40rpx;
	}

	.page-title {
		font-size: 30rpx;
		font-weight: 600;
		color: #000;
		height: 110rpx;
		display: flex;
		align-items: center;
	}

	.page-item {
		width: 33.3333%;
		text-align: center;
		padding-top: 40upx;
	}

	.page-image {
		width: 120upx;
		height: 120upx;
	}

	.g-title {
		font-size: 22upx;
	}

	.arrive-down {
		border: 0px solid black;
		color: rgb(155, 155, 155);
		/* position: relative; */
		box-sizing: border-box;
		display: flex;
		-webkit-box-orient: horizontal;
		flex-direction: row;
		place-content: flex-start center;
		flex-shrink: 0;
		-webkit-box-pack: center;
		-webkit-box-align: center;
		align-items: center;
		height: 80upx;
		margin-top: 20upx;
		margin-bottom: 54upx;
	}

	.arrive-down .left,
	.arrive-down .right {
		background-color: rgb(155, 155, 155);
		height: 2upx;
		width: 50upx;
		margin-left: 10upx;
		margin-right: 10upx;
	}
	.bottom-text {
		font-size: 22rpx;
	}
</style>
