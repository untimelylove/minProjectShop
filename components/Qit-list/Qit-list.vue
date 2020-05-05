<template>
	<view :style="'background:' + bg">
		<!-- 搜索条 -->
		<view class="cu-bar tabbar bg-white fixed" :style="[{top:CustomBar + 'px'}]">
			<view class="action">
				<view @click="synthesizeClick" class="" :class="index==1?'text-red':''" style=" dvertical-align: bottom;">
					<text class="text-df">{{syntText}}</text>
					<text class="iconfont" v-show="c">&#xe68e;</text>
					<text class="iconfont" v-show="!c">&#xe68d;</text>
				</view>
			</view>
			<view class="action">
				<view class="text-df" @click="salesClick" :class="index==2?'text-red':''">销量</view>
			</view>
			<view class="action">
				<view class="" @click="listTypeClick">
					<!-- 一行两个 -->
					<text v-if="isList==true" class="iconfont text-xxl ">&#xe7f9;</text>
					<!-- 一行一个 -->
					<text v-if="isList==false" class="iconfont text-xxl ">&#xe625;</text>
				</view>
			</view>
			<view class="action" @click="showFilter=true">
				<view class="text-df">更多<text class="iconfont">&#xe6a0;</text></view>
			</view>
		</view>
		<!-- 综合搜索栏选项 -->
		<view class="tools-alert" @click="c=!c" v-show="c" :style="[{top:(CustomBar+45)+'px'}]">
			<view class="cu-list menu text-left solid-top">
				<view class="cu-item solid-bottom" v-for="(item,i) in cList" :key="i" @click.stop="choose(item)">
					<view class="content padding-sm">
						<text class="">{{item}}</text>
					</view>
					<view v-if="item==syntText" class="action padding-sm">
						<text class="iconfont text-red">&#xe60b;</text>
					</view>
				</view>
			</view>
		</view>
		<!-- 过滤选项内容 -->
		<view class="cu-modal drawer-modal justify-end" style="overflow: hidden;" :class="showFilter?'show':''" @tap="showFilter=false">
			<view class="cu-dialog basis-xl" @tap.stop="" @touchmove.stop="" :style="[{top:CustomBar+'px',height:'calc(100vh - ' + CustomBar + 'px)'}]">
				<!-- :style="[{height:'calc(100vh - '+(CustomBar+120)+'px)'}]" -->
				<view class="filter-content">
					<view class="cu-bar bg-white ">
						<view class="action">
							<text class="text-blue"></text>类别
						</view>
					</view>
					<view class="bg-white solid-bottom">
						<button class="cu-btn bg-gray margin-right-sm margin-tb-sm">玉米</button>
						<button class="cu-btn bg-gray margin-right-sm margin-tb-sm">菠菜</button>
						<button class="cu-btn bg-gray margin-right-sm margin-tb-sm">土豆</button>
						<button class="cu-btn bg-gray margin-right-sm margin-tb-sm">黄瓜</button>
						<button class="cu-btn bg-gray margin-right-sm margin-tb-sm">玉米</button>
						<button class="cu-btn bg-gray margin-right-sm margin-tb-sm">菠菜</button>
						<button class="cu-btn bg-gray margin-right-sm margin-tb-sm">土豆</button>
						<button class="cu-btn bg-gray margin-right-sm margin-tb-sm">黄瓜</button>
						<button class="cu-btn bg-gray margin-right-sm margin-tb-sm">玉米</button>
						<button class="cu-btn bg-gray margin-right-sm margin-tb-sm">菠菜</button>
						<button class="cu-btn bg-gray margin-right-sm margin-tb-sm">土豆</button>
						<button class="cu-btn bg-gray margin-right-sm margin-tb-sm">黄瓜</button>
					</view>
					<view class="cu-bar bg-white ">
						<view class="action">
							<text class="text-blue"></text>价格
						</view>
					</view>
					<view class="bg-white solid-bottom">
						<view class="flex justify-center">
							<input type="digit" class="filter-price-input bg-gray" placeholder-class="placeholder-input" placeholder="最低价"></input>
							<view class="solids-center margin-lr"></view>
							<input type="digit" class="filter-price-input bg-gray" placeholder-class="placeholder-input" placeholder="最高价"></input>
						</view>
						<view class="flex">
							<view class="bg-gray flex-sub padding-sm margin-sm">
								<view>10-19</view>
								<view class="text-gray">15%选择</view>
							</view>
							<view class="bg-gray flex-sub padding-sm margin-sm">
								<view>19-31</view>
								<view class="text-gray">33%选择</view>
							</view>
							<view class="bg-gray flex-sub padding-sm margin-sm">
								<view>31-120</view>
								<view class="text-gray">35%选择</view>
							</view>
						</view>
					</view>
				</view>
				<view class="flex">
					<view class="flex-sub bg-gray padding-lg" @click="showFilter=false">
						<text class="iconfont">&#xe605;</text>
					</view>
					<view class="flex-twice bg-orange padding-lg">重置</view>
					<view class="flex-treble bg-red padding-lg">确认</view>
				</view>
			</view>
		</view>
		<view class="tui-product-list">
			<view class="tui-product-container">
				<scroll-view scroll-y scroll-with-animation>
					<block v-for="(item,index) in dataList" :key="index" v-if="(index+1)%2!=0 || isList">
						<!-- <template is="productItem" data="{{item,index:index,isList:isList}}" /> -->
						<!--商品列表-->
						<view class="tui-pro-item" :class="[isList?'tui-flex-list':'']" hover-class="hover" :hover-start-time="150" @tap="detail">
							<image :src="item.pic" class="tui-pro-img" :class="[isList?'tui-proimg-list':'']" mode="widthFix" />
							<view class="tui-pro-content">
								<view class="tui-pro-tit">{{item.name}}</view>
								<view>
									<view class="tui-pro-price">
										<text class="tui-sale-price">￥{{item.minPrice}}</text>
										<text class="tui-factory-price" v-if="item.originalPrice != 0">￥{{item.originalPrice}}</text>
									</view>
									<view class="tui-pro-pay cf">{{item.numberSells}}人付款
										<view class='line-tag text-xs fr line-orange radius' v-if="item.tags == '包邮'">{{item.tags}}</view>
										<view class='common-tag text-xs text-white fr radius' v-else>{{item.tags}}</view>
									</view>
								</view>
							</view>
						</view>
						<!--商品列表-->
					</block>
				</scroll-view>
			</view>
			<view class="tui-product-container" v-if="!isList">
				<scroll-view scroll-y scroll-with-animation>
					<block v-for="(item,index) in dataList" :key="index" v-if="(index+1)%2==0" @tap="shopDetail()">
						<!-- <template is="productItem" data="{{item,index:index}}" /> -->
						<!--商品列表-->
						<view class="tui-pro-item" :class="[isList?'tui-flex-list':'']" hover-class="hover" :hover-start-time="150" @tap="detail">
							<image :src="item.pic" class="tui-pro-img" :class="[isList?'tui-proimg-list':'']" mode="widthFix" />
							<view class="tui-pro-content">
								<view class="tui-pro-tit">{{item.name}}</view>
								<view>
									<view class="tui-pro-price">
										<text class="tui-sale-price">￥{{item.minPrice}}</text>
										<text class="tui-factory-price" v-if="item.originalPrice != 0">￥{{item.originalPrice}}</text>
									</view>
									<view class="tui-pro-pay cf">{{item.numberSells}}人付款
										<view class='line-tag text-xs fr line-orange radius' v-if="item.tags == '包邮'">{{item.tags}}</view>
										<view class='common-tag text-xs text-white fr radius' v-else>{{item.tags}}</view>
									</view>
								</view>
							</view>
						</view>
						<!--商品列表-->
					</block>
				</scroll-view>
			</view>
		</view>
		</scroll-view>
	</view>
</template>

<script>
	export default {
		// props:["dataList"],
		props: {
			dataList: { // 数据列表
				type: Array,
				default () {
					return []
				}
			},
		},
		data() {
			return {
				CustomBar: 44,
				StatusBar: this.StatusBar,
				/* 搜索条默认选中项 */
				index: 1,
				/* 搜索条综合是否展开 */
				c: false,
				/* 搜索条综合搜索选项 */
				cList: ['综合', '最新上架', '价格最低', '价格最高', '评价最多'],
				/* 搜索条 综合显示文本 */
				syntText: '综合',
				/* 列表类型 1一行一个，2一行两个*/
				isList: false, //是否以列表展示: 1,
				/* 更多过滤 */
				showFilter: false,
				dropScreenH: 0,
				bg: '#f7f7f7' //列表背景颜色
			}
		},
		methods: {
			// 商品详情
			detail() {
				console.log('111111111')
				uni.navigateTo({
					url:'../../pages/shopDetail/shopDetail'
				})
			},
			/* 点击综合 */
			synthesizeClick() {
				this.c = !this.c
			},
			/* 点击销量 */
			salesClick() {
				this.index = 2
			},
			/* 综合 点击选项 */
			choose(item) {
				this.syntText = item
				this.c = false
				this.index = 1
			},
			/*列表类型点击  */
			listTypeClick() {
				this.isList = this.isList == true ? false : true
				if (this.isList) {
					console.log('111111')
					this.bg = '#ffffff'
				} else {
					this.bg = '#f7f7f7'
				}
			},
		}

	}
</script>

<style>
	page {
		/* background-color: #FFFFFF; */
	}

	.tools-alert {
		height: 100%;
		position: fixed;
		width: 100%;
		left: 0;
		box-sizing: border-box;
		background-color: rgba(0, 0, 0, 0.4);
		z-index: 100;
	}

	.filter-content {
		overflow-y: auto;
		/* #ifdef H5 */
		height: calc(100vh - 105px);
		/* #endif */
		/* #ifdef MP-WEIXIN */
		height: calc(100vh - 125px);
		/* #endif */
		/* #ifdef APP-VUE */
		height: calc(100vh - 145px);
		/* #endif */
	}

	.filter-price-input {
		width: 232upx;
		height: 80upx;
		border-radius: 6upx;
	}

	.solids-center {
		height: 4upx;
		background-color: #eee;
		width: 30upx;
		margin-top: 38upx;
	}

	/* 商品列表*/

	.tui-product-list {
		display: flex;
		justify-content: space-between;
		flex-direction: row;
		flex-wrap: wrap;
		box-sizing: border-box;
		margin: 100rpx 20rpx 0;
		/* margin-left: 20rpx; */
		/* margin-right: 20rpx; */
	}

	.tui-product-container {
		flex: 1;
		margin-right: 15rpx;
	}

	.tui-product-container:last-child {
		margin-right: 0;
	}

	.tui-pro-item {
		width: 100%;
		margin-bottom: 16rpx;
		background: #fff;
		box-sizing: border-box;
		border-radius: 18rpx;
		overflow: hidden;
		transition: all 0.15s ease-in-out;
	}

	.tui-flex-list {
		display: flex;
		margin-bottom: 20rpx !important;
	}

	.tui-pro-img {
		width: 100%;
		display: block;
	}

	.tui-proimg-list {
		width: 260rpx;
		height: 260rpx !important;
		flex-shrink: 0;
		border-radius: 12rpx;
	}

	.tui-pro-content {
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		box-sizing: border-box;
		padding: 20rpx;
	}

	.tui-pro-tit {
		color: #2e2e2e;
		font-size: 26rpx;
		word-break: break-all;
		overflow: hidden;
		text-overflow: ellipsis;
		display: -webkit-box;
		-webkit-box-orient: vertical;
		-webkit-line-clamp: 2;
	}

	.tui-pro-price {
		padding-top: 18rpx;
	}

	.tui-sale-price {
		font-size: 34rpx;
		font-weight: 500;
		color: #e41f19;
	}

	.tui-factory-price {
		font-size: 24rpx;
		color: #a0a0a0;
		text-decoration: line-through;
		padding-left: 12rpx;
	}

	.tui-pro-pay {
		padding-top: 10rpx;
		font-size: 24rpx;
		color: #999;
	}

	.common-tag {
		vertical-align: middle;
		position: relative;
		display: -webkit-inline-box;
		display: -webkit-inline-flex;
		display: inline-flex;
		-webkit-box-align: center;
		-webkit-align-items: center;
		align-items: center;
		-webkit-box-pack: center;
		-webkit-justify-content: center;
		justify-content: center;
		-webkit-box-sizing: border-box;
		box-sizing: border-box;
		padding: 0rpx 10rpx;
		height: 34rpx;
		font-family: Helvetica Neue, Helvetica, sans-serif;
		white-space: nowrap;
		background-color: #F91919;
	}

	.line-tag {
		vertical-align: middle;
		position: relative;
		display: -webkit-inline-box;
		display: -webkit-inline-flex;
		display: inline-flex;
		-webkit-box-align: center;
		-webkit-align-items: center;
		align-items: center;
		-webkit-box-pack: center;
		-webkit-justify-content: center;
		justify-content: center;
		-webkit-box-sizing: border-box;
		box-sizing: border-box;
		padding: 0rpx 10rpx;
		height: 34rpx;
		font-family: Helvetica Neue, Helvetica, sans-serif;
		white-space: nowrap;
		border: 1rpx solid #f37b1d;
	}

	/* 商品列表*/
</style>
