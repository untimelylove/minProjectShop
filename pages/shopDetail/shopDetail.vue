<template>
	<view>
		<!--header-->
		<view class="cu-custom">
			<view class="cu-bar fixed none-bg text-black" :style="{height:CustomBar + 'px',background:'rgba(255,255,255,'+opcity+')'}">
				<view class="content" :style="[{top:StatusBar + 'px',opacity:opcity}]">
					商品详情
				</view>
				<view class="tui-header-icon" :style="{marginTop:StatusBar+'px'}">
					<view class="tui-icon tui-icon-arrowleft tui-icon-back" :style="{color:opcity>=1?'#000':'#fff',background:'rgba(0, 0, 0,'+iconOpcity+')'}"
					 @tap="back"></view>
					<view class="tui-icon tui-icon-more-fill  tui-icon-ml" :style="{color:opcity>=1?'#000':'#fff',background:'rgba(0, 0, 0,'+iconOpcity+')',fontSize:'20px'}"
					 @tap.stop="openMenu"></view>
					<tui-badge type="red" size="small">5</tui-badge>
				</view>
			</view>
		</view>
		<!--header-->
		<!--banner-->
		<view class="tui-banner-swiper">
			<swiper :autoplay="true" :interval="5000" :duration="150" :circular="true" :style="{height:scrollH + 'px'}" @change="bannerChange">
				<block v-for="(item,index) in banner" :key="index">
					<swiper-item :data-index="index" @tap.stop="previewImage">
						<image :src="item" class="tui-slide-image" :style="{height:scrollH+'px'}" />
					</swiper-item>
				</block>
			</swiper>
			<tui-tag type="translucent" shape="circleLeft" size="small">{{bannerIndex+1}}/{{banner.length}}</tui-tag>
		</view>
		<!--banner-->
		<view class="tui-pro-detail">
			<!-- 价格介绍 -->
			<view class="tui-product-title tui-border-radius">
				<view class="tui-pro-pricebox tui-padding">
					<view class="tui-pro-price">
						<view>￥<text class="tui-price">6488</text>.00</view>
						<tui-tag size="small" :plain="true" type="high-green" shape="circle">新品</tui-tag>
					</view>
					<view class="tui-collection tui-size" @tap="collecting">
						<view class="tui-icon tui-icon-collection" :class="['tui-icon-'+(collected?'like-fill':'like')]" :style="{color:collected?'#ff201f':'#333',fontSize:'20px'}"></view>
						<view class="tui-scale" :class="[collected?'tui-icon-red':'']">2.2万</view>
					</view>
				</view>
				<view class="tui-original-price tui-gray">
					价格
					<text class="tui-line-through">￥6688.00</text>
				</view>
				<view class="tui-pro-titbox">
					<view class="tui-pro-title">
						<view class='cu-tag radius bg-jdred margin-right-sm sm'>自营</view>华为 HUAWEI P40 Pro 麒麟990 5G SoC芯片
						5000万超感知徕卡四摄 50倍数字变焦 8GB+256GB亮黑色全网通5G手机
					</view>
					<button open-type="share" class="tui-share-btn tui-share-position">
						<tui-tag type="gray" tui-tag-class="tui-tag-share tui-size" shape="circleLeft" size="small">
							<view class="tui-icon tui-icon-partake" style="color:#999;font-size:15px"></view>
							<!-- <tui-icon name="partake" color="#999" size="15"></tui-icon> -->
							<text class="tui-share-text tui-gray">分享</text>
						</tui-tag>
					</button>
				</view>
				<view class="tui-padding">
					<view class="tui-sub-title tui-size tui-gray">【以旧换新至高贴1000元】5000万超感知徕卡四摄；买赠好礼Mate30直降500元享12期》 </view>
					<view class="tui-sale-info tui-size tui-gray">
						<view>快递：0.00</view>
						<view>月销2000</view>
						<view>浙江杭州</view>
					</view>
				</view>
			</view>
			<!-- 价格介绍 -->
			<!-- 中部选项 -->
			<view class="tui-discount-box tui-radius-all tui-mtop">
				<view class="tui-list-cell tui-last" @tap="showPopup">
					<view class="tui-bold tui-cell-title">优惠</view>
					<view>
						<view class="tui-promotion-box">
							<view class='cu-tag line-red margin-right-sm sm'>换购</view>
							<text>购买1件可优惠换购热销商品</text>
						</view>
						<view class="tui-promotion-box">
							<view class='cu-tag line-red margin-right-sm sm'>免费领</view>
							<text>体验卡免费领，超大流量</text>
						</view>
						<view class="tui-promotion-box">
							<view class='cu-tag line-red margin-right-sm sm'>优惠套装</view>
							<text>该商品共有4款优惠套装</text>
						</view>
					</view>
					<tui-icon name="more-fill" :size="20" class="tui-right tui-top40" color="#666"></tui-icon>
				</view>
			</view>
			<view class="tui-basic-info tui-mtop tui-radius-all">
				<view class="tui-list-cell" @tap="showPopup">
					<view class="tui-bold tui-cell-title">已选</view>
					<view class="tui-selected-box">{{selecePhone}}</view>
					<tui-icon name="more-fill" :size="20" class="tui-right" color="#666"></tui-icon>
				</view>
				<view class="tui-list-cell" @tap="showPopup">
					<view class="tui-bold tui-cell-title">送至</view>
					<view class="tui-addr-box">
						<view class="tui-addr-item">北京朝阳区三环到四环之间</view>
						<view class="tui-addr-item text-grey">今日23:59前完成下单，预计6月28日23:30前发货，7月1日24:00前送达</view>
					</view>
					<tui-icon name="more-fill" :size="20" class="tui-right" color="#666"></tui-icon>
				</view>
				<view class="tui-list-cell tui-last">
					<view class="tui-bold tui-cell-title">重量</view>
					<view class="tui-selected-box">0.55kg</view>
				</view>
				<view class="tui-guarantee">
					<view class="tui-guarantee-item">
						<tui-icon name="circle-selected" :size="14" color="#F3523C"></tui-icon>
						<text class="tui-pl">99元免基础运费(20kg内)</text>
					</view>
					<view class="tui-guarantee-item">
						<tui-icon name="circle-selected" :size="14" color="#F3523C"></tui-icon>
						<text class="tui-pl">店铺发货&售后</text>
					</view>
					<view class="tui-guarantee-item">
						<tui-icon name="circle-selected" :size="14" color="#F3523C"></tui-icon>
						<text class="tui-pl">7天无理由退货</text>
					</view>
					<view class="tui-guarantee-item">
						<tui-icon name="circle-selected" :size="14" color="#F3523C"></tui-icon>
						<text class="tui-pl">闪电退款</text>
					</view>
					<view class="tui-guarantee-item">
						<tui-icon name="circle-selected" :size="14" color="#F3523C"></tui-icon>
						<text class="tui-pl">极速审核</text>
					</view>
				</view>
			</view>
			<!-- 中部选项 -->
			<!-- 底部弹框 -->
			<tui-bottom-popup :show="popupShow" @close="hidePopup">
				<view class="tui-popup-box">
					<view class="tui-product-box tui-padding">
						<image src="https://www.thorui.cn/img/product/11.jpg" class="tui-popup-img"></image>
						<view class="tui-popup-price">
							<view class="tui-amount tui-bold">￥6488.00</view>
							<view class="tui-number">编号:4373299399393</view>
						</view>
					</view>
					<scroll-view scroll-y class="tui-popup-scroll">
						<view class="tui-scrollview-box">
							<view class="tui-bold tui-attr-title">颜色</view>
							<view class="tui-attr-box">
								<view class="tui-attr-item" v-for="(color,index) in phoneColor" :key="index" :class="colorActive === index? 'tui-attr-active':''"
								 @tap="colorTab(index,color)">
									{{color}}
								</view>
							</view>
							<view class="tui-bold tui-attr-title">版本</view>
							<view class="tui-attr-box">
								<view class="tui-attr-item" v-for="(ram,index) in phoneRam" :key="index" :class="ramActive === index? 'tui-attr-active':''"
								 @tap="ramTab(index,ram)">
									{{ram}}
								</view>
							</view>
							<view class="tui-number-box tui-bold tui-attr-title">
								<view class="tui-attr-title">数量</view>
								<tui-numberbox :max="99" :min="1" :value="value" @change="change"></tui-numberbox>
							</view>
							<view class="tui-bold tui-attr-title">
								保障服务
							</view>
							<view class="tui-attr-box">
								<view class="tui-attr-item" v-for="(tee,index) in guarantee" :key="index" :class="teeActive === index? 'tui-attr-active':''"
								 @tap="teeTab(index)">
									{{tee}}
								</view>
							</view>

							<view class="tui-bold tui-attr-title">
								屏幕保护
							</view>
							<view class="tui-attr-box">
								<view class="tui-attr-item" v-for="(tect,index) in protect" :key="index" :class="tectActive === index? 'tui-attr-active':''"
								 @tap="tectTab(index)">
									{{tect}}
								</view>
							</view>
						</view>
					</scroll-view>
					<view class="tui-operation tui-operation-right tui-right-flex tui-popup-btn">
						<view class="tui-flex-1">
							<tui-button type="red" shape="circle" size="mini" @click="hidePopup">加入购物车</tui-button>
						</view>
						<view class="tui-flex-1">
							<tui-button type="warning" shape="circle" size="mini" @click="submit">立即购买</tui-button>
						</view>
					</view>
					<view class="tui-icon tui-icon-close-fill tui-icon-close" style="color: #999;font-size:20px" @tap="hidePopup"></view>
					<!-- <tui-icon name="close-fill" color="#999" class="tui-icon-close" size="20" @tap="hidePopup"></tui-icon> -->
				</view>
			</tui-bottom-popup>
			<!-- 底部弹框 -->
		</view>
	</view>
</template>

<script>
	const WXAPI = require('apifm-wxapi')
	import tuiIcon from "@/components/icon/icon"
	import tuiTag from "@/components/tag/tag"
	import tuiBottomPopup from "@/components/bottom-popup/bottom-popup"
	import tuiButton from "@/components/button/button"
	import tuiNumberbox from "@/components/numberbox/numberbox"
	export default {
		data() {
			return {
				CustomBar: this.CustomBar,
				StatusBar: this.StatusBar,
				// height: 64, //header高度
				// top: 0, //标题图标距离顶部距离
				scrollH: 0, //滚动总高度
				opcity: 0,
				iconOpcity: 0.5,
				banner: [
					"https://res.vmallres.com/pimages//product/6901443381517/428_428_0410C56E4DFF825A527CBD3369F4D394F1699311CF51F864mp.png",
					"https://res.vmallres.com/pimages//product/6901443381517/group//428_428_22CAF23AA6BD8E6117D36D479FCEFCF061371FCF2AC8C174.png",
					"https://res.vmallres.com/pimages//product/6901443381517/group//428_428_ED1024583ED2E831CF2A474558A0DCDF7651CEBF5C319C46.png",
					"https://res.vmallres.com/pimages//product/6901443381517/group//428_428_22CAF23AA6BD8E6117D36D479FCEFCF061371FCF2AC8C174.png",
					"https://res.vmallres.com/pimages//product/6901443381517/group//428_428_671327846CDBA41AE5512C4F516C54AC57B62FDEED33C29D.png",
					"https://res.vmallres.com/pimages//product/6901443381517/group//428_428_A6A55F1C179B9AD34FEB2A7D49370DAD95BF6B400A39B780.png",
					"https://res.vmallres.com/pimages//product/6901443381517/group//428_428_892E56E53608D0663B0CAB3E706C5EF737290A529966A128.png",
				],
				phoneColor: ['亮黑色', '冰霜银', '零度白', '深海蓝'],
				phoneRam: ['8+128G全网通', '8+256G全网通', '6+128G全网通'],
				guarantee: ['2年全保修 ￥269.00', '3年全保修 ￥399.00'],
				protect: ['1年碎屏保修￥159.00', '2年碎屏保修￥179.00'],
				bannerIndex: 0,
				collected: false,
				popupShow: false,
				value: 1,
				selecePhone: '亮黑色，8+128G全网通，1个',
				colorActive: -1,
				coloragain: true,
				ramActive: -1,
				ramagain: true,
				teeActive: -1,
				teeagain: true,
				tectActive: -1,
				tectagain: true,
				selectColor: '',
				selectRam: '',
				active: ''
			}
		},
		computed: {
			style() {
				var StatusBar = this.StatusBar;
				var CustomBar = this.CustomBar;
				var bgImage = this.bgImage;
				var style = `height:${CustomBar}px;padding-top:${StatusBar}px;`;
				if (this.bgImage) {
					style = `${style}background-image:url(${bgImage});`;
				}
				return style
			}
		},
		components: {
			tuiIcon,
			tuiTag,
			// tuiBadge,
			// tuiNomore,
			tuiButton,
			// tuiTopDropdown,
			tuiBottomPopup,
			tuiNumberbox
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
			this.getRegion()
			setTimeout(() => {
				uni.getSystemInfo({
					success: (res) => {
						this.width = obj.left || res.windowWidth;
						this.height = obj.top ? (obj.top + obj.height + 8) : (res.statusBarHeight + 44);
						this.top = obj.top ? (obj.top + (obj.height - 32) / 2) : (res.statusBarHeight + 6);
						this.scrollH = res.windowWidth
					}
				})
			}, 50)
		},
		methods: {
			// 商品详情
			// getShopDetail() {
			// 	WXAPI.goodsDetail(this.id).then(res => {
			// 		if (res.code == 0) {
			// 			// this.banner = res.data.pics
			// 			this.detail = res.data
			// 		}
			// 	})
			// },
			// 获取省份
			getRegion() {
				WXAPI.province().then(res => {
					console.log(res)

				}).catch(err => {
					console.log(err)
				})
			},
			back: function() {
				uni.navigateBack()
			},
			openMenu: function() {
				this.menuShow = true
			},
			previewImage: function(e) {
				let index = e.currentTarget.dataset.index;
				uni.previewImage({
					current: this.banner[index],
					urls: this.banner
				})
			},
			bannerChange: function(e) {
				this.bannerIndex = e.detail.current
			},
			collecting: function() {
				this.collected = !this.collected
			},
			showPopup: function() {
				this.popupShow = true
			},
			hidePopup: function() {
				if (this.selectColor == '' && this.selectRam == '') {
					this.popupShow = false
				} else {
					this.selecePhone = this.selectColor + '，' + this.selectRam
					this.popupShow = false
				}
			},
			colorTab(index, color) {
				if (this.coloragain == false && this.colorActive == index) {
					this.colorActive = -1
				} else {
					this.colorActive = index
					this.coloragain = false
					this.selectColor = color
				}
			},
			ramTab(index, ram) {
				if (this.ramagain == false && this.ramActive == index) {
					this.ramActive = -1
				} else {
					this.ramActive = index
					this.ramagain = false
					this.selectRam = ram
				}
			},
			teeTab(index) {
				if (this.teeagain == false && this.teeActive == index) {
					this.teeActive = -1
				} else {
					this.teeActive = index
					this.teeagain = false
				}
			},
			tectTab(index) {
				if (this.tectagain == false && this.tectActive == index) {
					this.tectActive = -1
				} else {
					this.tectActive = index
					this.tectagain = false
				}
			},
			submit() {
				this.popupShow = false
				uni.navigateTo({
					url: '../mall-extend/submitOrder/submitOrder'
				})
			},
			change: function(e) {
				this.value = e.value
			},
		},
		onPageScroll(e) {
			let scroll = e.scrollTop <= 0 ? 0 : e.scrollTop;
			let opcity = scroll / this.scrollH;
			if (this.opcity >= 1 && opcity >= 1) {
				return;
			}
			this.opcity = opcity;
			this.iconOpcity = 0.5 * (1 - opcity < 0 ? 0 : 1 - opcity)
		}
	}
</script>

<style>
	/* icon样式 */
	@import "../../static/style/icon.css";

	page {
		background: #f7f7f7;
	}

	.container {
		padding-bottom: 110rpx;
	}

	.tui-header-box {
		width: 100%;
		position: fixed;
		left: 0;
		top: 0;
		z-index: 9998;
	}

	.tui-header {
		width: 100%;
		font-size: 18px;
		line-height: 18px;
		font-weight: 500;
		height: 32px;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.tui-header-icon {
		position: fixed;
		top: 0;
		left: 10px;
		display: flex;
		align-items: flex-start;
		justify-content: space-between;
		height: 32px;
		transform: translateZ(0);
		z-index: 99999;
	}



	.tui-header-icon .tui-badge {
		background: #e41f19 !important;
		position: absolute;
		right: -4px;
	}

	.tui-icon-ml {
		margin-left: 20rpx;
	}

	.tui-icon {
		border-radius: 16px;
	}


	.tui-icon-back {
		height: 32px !important;
		width: 32px !important;
		display: block !important;
	}

	.tui-header-icon .tui-icon-more-fill {
		height: 20px !important;
		width: 20px !important;
		padding: 6px !important;
		display: block !important;
	}

	.tui-banner-swiper {
		position: relative;
	}

	.tui-banner-swiper .tui-tag-class {
		position: absolute;
		color: #fff;
		bottom: 30rpx;
		right: 0;
	}

	.tui-slide-image {
		width: 100%;
		display: block;
	}

	/*顶部菜单*/

	.tui-menu-box {
		box-sizing: border-box;
	}

	.tui-menu-header {
		font-size: 34rpx;
		color: #fff;
		height: 32px;
		display: flex;
		align-items: center;
	}

	.tui-top-dropdown {
		z-index: 9999 !important;
	}

	.tui-menu-itembox {
		color: #fff;
		padding: 40rpx 10rpx 0 10rpx;
		box-sizing: border-box;
		display: flex;
		flex-wrap: wrap;
		font-size: 26rpx;
	}

	.tui-menu-item {
		width: 22%;
		height: 160rpx;
		border-radius: 24rpx;
		display: flex;
		align-items: center;
		flex-direction: column;
		justify-content: center;
		background: rgba(0, 0, 0, 0.4);
		margin-right: 4%;
		margin-bottom: 4%;
	}

	.tui-menu-item:nth-of-type(4n) {
		margin-right: 0;
	}

	.tui-badge-box {
		position: relative;
	}

	.tui-badge-box .tui-badge-class {
		position: absolute;
		top: -8px;
		right: -8px;
	}

	.tui-msg-badge {
		top: -10px;
	}

	.tui-icon-up {
		position: relative;
		display: inline-block;
		left: 50%;
		transform: translateX(-50%);
	}

	.tui-menu-text {
		padding-top: 12rpx;
	}

	.tui-opcity .tui-menu-text,
	.tui-opcity .tui-badge-box {
		opacity: 0.5;
		transition: opacity 0.2s ease-in-out;
	}

	/*顶部菜单*/

	/*内容 部分*/

	.tui-padding {
		padding: 0 30rpx;
		box-sizing: border-box;
	}

	/* #ifdef H5 */
	.tui-ptop {
		padding-top: 44px;
	}

	/* #endif */

	.tui-size {
		font-size: 24rpx;
		line-height: 24rpx;
	}

	.tui-gray {
		color: #999;
	}

	.tui-icon-red {
		color: #ff201f;
	}

	.tui-border-radius {
		border-bottom-left-radius: 24rpx;
		border-bottom-right-radius: 24rpx;
		overflow: hidden;
	}

	.tui-radius-all {
		border-radius: 24rpx;
		overflow: hidden;
	}

	.tui-mtop {
		margin-top: 26rpx;
	}

	.tui-pro-detail {
		box-sizing: border-box;
		color: #333;
	}

	.tui-product-title {
		background: #fff;
		padding: 30rpx 0;
	}

	.tui-pro-pricebox {
		display: flex;
		align-items: center;
		justify-content: space-between;
		color: #f2270c;
		font-size: 36rpx;
		font-weight: bold;
		line-height: 44rpx;
	}

	.tui-pro-price {
		display: flex;
		align-items: center;
	}

	.tui-pro-price .tui-tag-class {
		transform: scale(0.7);
		transform-origin: center center;
		line-height: 24rpx;
		font-weight: normal;
	}

	.tui-price {
		font-size: 58rpx;
	}

	.tui-original-price {
		font-size: 26rpx;
		line-height: 26rpx;
		padding: 10rpx 30rpx;
		box-sizing: border-box;
	}

	.tui-line-through {
		text-decoration: line-through;
	}

	.tui-collection {
		color: #333;
		display: flex;
		align-items: center;
		flex-direction: column;
		justify-content: center;
		height: 44rpx;
	}

	.tui-scale {
		transform: scale(0.7);
		transform-origin: center center;
		line-height: 24rpx;
		font-weight: normal;
	}

	.tui-icon-collection {
		line-height: 20px !important;
		margin-bottom: 0 !important;

	}

	.tui-pro-titbox {
		font-size: 32rpx;
		font-weight: 500;
		position: relative;
		padding: 0 150rpx 0 30rpx;
		box-sizing: border-box;
		color: #262626;

	}

	.tui-pro-title {
		padding-top: 20rpx;
		font-weight: bold;
	}

	.tui-share-btn {
		display: block;
		background: none;
		margin: 0;
		padding: 0;
		border-radius: 0;
	}

	.tui-tag-share {
		display: flex;
		align-items: center;
	}

	.tui-share-position {
		position: absolute;
		right: 0;
		top: 30rpx;
	}

	.tui-share-text {
		padding-left: 8rpx;
	}

	.tui-sub-title {
		padding: 20rpx 0;
		line-height: 40rpx;
	}

	.tui-sale-info {
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding-top: 10rpx;
	}

	.tui-discount-box {
		background: #fff;
	}

	.tui-list-cell {
		position: relative;
		display: flex;
		align-items: center;
		font-size: 26rpx;
		line-height: 26rpx;
		padding: 36rpx 30rpx;
		box-sizing: border-box;
	}

	.tui-right {
		position: absolute;
		right: 30rpx;
		top: 30rpx;
	}

	.tui-top40 {
		top: 40rpx !important;
	}

	.tui-bold {
		font-weight: bold;
	}

	.tui-list-cell::after {
		content: '';
		position: absolute;
		border-bottom: 1rpx solid #eaeef1;
		-webkit-transform: scaleY(0.5);
		transform: scaleY(0.5);
		bottom: 0;
		right: 0;
		left: 126rpx;
	}

	.tui-last::after {
		border-bottom: 0 !important;
	}

	.tui-tag-coupon-box {
		display: flex;
		align-items: center;
	}

	.tui-tag-coupon-box .tui-tag-class {
		margin-right: 20rpx;
	}


	.tui-cell-title {
		/* width: 66rpx; 造成字体被挤竖起来放着 */
		padding-right: 30rpx;
		flex-shrink: 0;
	}

	.tui-promotion-box {
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
		padding: 10rpx 0;
		width: 100%;
	}

	.tui-promotion-box .tui-tag-class {
		display: inline-block !important;
		transform: scale(0.8);
		transform-origin: 0 center;
	}

	/* .tui-inline-block {
		display: inline-block !important;
		transform: scale(0.8);
		transform-origin: 0 center;
	} */

	.tui-basic-info {
		background: #fff;
	}

	.tui-addr-box {
		width: 76%;
	}

	.tui-addr-item {
		padding: 10rpx 0;
		line-height: 34rpx;
	}

	.tui-guarantee {
		background: #fdfdfd;
		display: flex;
		flex-wrap: wrap;
		padding: 20rpx 30rpx 30rpx 30rpx;
		font-size: 24rpx;
	}

	.tui-guarantee-item {
		color: #999;
		padding-right: 30rpx;
		padding-top: 10rpx;
	}

	.tui-pl {
		padding-left: 4rpx;
	}

	.tui-cmt-box {
		background: #fff;
	}

	.tui-between {
		justify-content: space-between !important;
	}

	.tui-cmt-all {
		color: #ff201f;
		padding-right: 8rpx;
	}

	.tui-cmt-content {
		font-size: 26rpx;
	}

	.tui-cmt-user {
		display: flex;
		align-items: center;
	}

	.tui-acatar {
		width: 60rpx;
		height: 60rpx;
		border-radius: 30rpx;
		display: block;
		margin-right: 16rpx;
	}

	.tui-cmt {
		padding: 14rpx 0;
	}

	.tui-attr {
		font-size: 24rpx;
		color: #999;
		padding: 6rpx 0;
	}

	.tui-cmt-btn {
		padding: 50rpx 0 30rpx 0;
		box-sizing: border-box;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.tui-tag-cmt {
		min-width: 130rpx;
		padding: 20rpx 52rpx !important;
		font-size: 26rpx !important;
		display: inline-block;
	}

	.tui-nomore-box {
		padding-top: 10rpx;
	}

	.tui-product-img {
		transform: translateZ(0);
	}

	.tui-product-img image {
		width: 100%;
		display: block;
	}

	/*底部操作栏*/

	.tui-col-7 {
		width: 58.33333333%;
	}

	.tui-col-5 {
		width: 41.66666667%;
	}

	.tui-operation {
		width: 100%;
		height: 100rpx;
		/* box-sizing: border-box; */
		background: rgba(255, 255, 255, 0.98);
		position: fixed;
		display: flex;
		align-items: center;
		justify-content: space-between;
		z-index: 10;
		bottom: 0;
		left: 0;
		padding-bottom: env(safe-area-inset-bottom);
	}

	.tui-safearea-bottom {
		width: 100%;
		height: env(safe-area-inset-bottom);
	}

	.tui-operation::before {
		content: '';
		position: absolute;
		top: 0;
		right: 0;
		left: 0;
		border-top: 1rpx solid #eaeef1;
		-webkit-transform: scaleY(0.5);
		transform: scaleY(0.5);
	}

	.tui-operation-left {
		display: flex;
		align-items: center;
	}

	.tui-operation-item {
		flex: 1;
		display: flex;
		align-items: center;
		justify-content: center;
		flex-direction: column;
		position: relative;
	}

	.tui-operation-text {
		font-size: 22rpx;
		color: #333;
	}

	.tui-opacity {
		opacity: 0.5;
	}

	.tui-scale-small {
		transform: scale(0.9);
		transform-origin: center center;
	}

	.tui-operation-right {
		height: 100rpx;
		/* box-sizing: border-box; */
		padding-top: 0;
	}

	.tui-right-flex {
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.tui-btnbox-4 .tui-btn-class {
		width: 90% !important;
		display: block !important;
		font-size: 28rpx !important;
	}

	.tui-operation .tui-badge-class {
		position: absolute;
		top: -6rpx;
		/* #ifdef H5 */
		transform: translateX(50%)
			/* #endif  */
	}

	.tui-flex-1 {
		flex: 1;
	}

	/*底部操作栏*/

	/*底部选择弹层*/

	.tui-popup-class {
		border-top-left-radius: 24rpx;
		border-top-right-radius: 24rpx;
		padding-bottom: env(safe-area-inset-bottom);
	}

	.tui-popup-box {
		position: relative;
		padding: 30rpx 0 100rpx 0;
	}

	.tui-popup-btn {
		width: 100%;
		position: absolute;
		left: 0;
		bottom: 0;
	}

	.tui-popup-btn .tui-btn-class {
		width: 90% !important;
		display: block !important;
		font-size: 28rpx !important;
	}

	.tui-icon-close {
		position: absolute;
		top: 30rpx;
		right: 30rpx;
	}

	.tui-product-box {
		display: flex;
		align-items: flex-end;
		font-size: 24rpx;
		padding-bottom: 30rpx;
	}

	.tui-popup-img {
		height: 200rpx;
		width: 200rpx;
		border-radius: 24rpx;
		display: block;
	}

	.tui-popup-price {
		padding-left: 20rpx;
		padding-bottom: 8rpx;
	}

	.tui-amount {
		color: #ff201f;
		font-size: 36rpx;
	}

	.tui-number {
		font-size: 24rpx;
		line-height: 24rpx;
		padding-top: 12rpx;
		color: #999;
	}

	.tui-popup-scroll {
		height: 600rpx;
		font-size: 26rpx;
	}

	.tui-scrollview-box {
		padding: 0 30rpx 60rpx 30rpx;
		box-sizing: border-box;
	}

	.tui-attr-title {
		padding: 10rpx 0;
		color: #333;
	}

	.tui-attr-box {
		font-size: 0;
		padding: 20rpx 0;
	}

	.tui-attr-item {
		max-width: 100%;
		min-width: 200rpx;
		height: 64rpx;
		display: -webkit-inline-flex;
		display: inline-flex;
		align-items: center;
		justify-content: center;
		background: #f7f7f7;
		padding: 0 26rpx;
		box-sizing: border-box;
		border-radius: 32rpx;
		margin-right: 20rpx;
		margin-bottom: 20rpx;
		font-size: 26rpx;
	}

	.tui-attr-active {
		background: #fcedea !important;
		color: #e41f19;
		font-weight: bold;
		position: relative;
	}

	.tui-attr-active::after {
		content: "";
		position: absolute;
		border: 1rpx solid #e41f19;
		width: 100%;
		height: 100%;
		border-radius: 40rpx;
		left: 0;
		top: 0;
	}

	.tui-number-box {
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 20rpx 0 30rpx 0;
		box-sizing: border-box;
	}

	/*底部选择弹层*/
</style>
