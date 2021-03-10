<template>
	<view class="">
		<view class="header-cate">
			<scroll-view class="menus" scroll-x="true">
				<view v-for="(menu, indexs) in categories" :key="indexs" :class="[indexs == currentIndex ? 'menuActive' : '']" @tap="taps(indexs)">
					<!-- <navigator url="../pages/scroll-view">{{menu}}</navigator> -->
					{{menu.name}}
				</view>
			</scroll-view>
		</view>
		<view class="swiper-item center uni-h3 uni-link">
			<navigator url="../pages/scroll-view" animation-type="none">文档</navigator>
		</view>
	</view>
</template>

<script>
	var _this;
	export default {
		data: {
			currentIndex: 0,
			categories:[
				{cateId : 0, name : "热点推荐"},
				{cateId : 1, name : "uniapp文档"},
				{cateId : 2, name : "uniapp教程"},
				{cateId : 3, name : "demo"},
				{cateId : 4, name : "前端开发"},
			],
		},
		onLoad: function(options) {
			console.log("onLoad");
			_this = this;
		},
		onHide: function() {
			console.log("onHide");
		},
		onShow: function() {
			console.log("onShow");
		},
		methods: {
			taps(e) {
				console.log(e)
				_this.currentIndex = e;
			}
		}
	}
</script>

<style>
	/* 	
	.menus {
		white-space: nowrap;
		width: 100%;
	}
	.menus view {
		display: inline-block;
		width:100upx;
		height: 80upx;
		line-height: 80upx;
		text-align: center;
		font-size: 36upx;
	}

	.menuActive {
		color: #900;
		border-bottom: 2px solid #900;
	} */

	/* 首页分段器 */
	.header-cate {
		width: 100%;
		height: 42px;
		background: #FFF;
		border-bottom: 2px solid #F0F0F0;
		/* position: fixed; */
		/* z-index: 1; */
		/* left: 0; */
		/* top: 100upx; */
	}

	/* 头部滚蛋菜单 */
	.menus {
		white-space: nowrap;
		text-align: center;
	}

	.menus view {
		width: auto;
		padding: 0 12px;
		margin: 0 8px;
		line-height: 42px;
		display: inline-block;
		text-align: center;
		font-size: 30upx;
	}

	.menus view:first-child {
		margin-left: 0;
	}

	.menus view:last-child {
		margin-right: 0;
	}

	.menuActive {
		border-bottom: 2px solid #DE533A !important;
		color: #DE533A;
	}

	/* 文字相关 */
	.center {
		justify-content: center;
		text-align: center;
	}
</style>
