<template>
<view>
 	<!-- 	<image src="../../static/yuyin.svg" @click="input" mode="aspectFit" class="voice-icon"></image>
 		 <view class="search" @click="input">
			请输入关键词搜索
		</view> 
 		<image src="../../static/sousuo.svg" @click="input" mode="aspectFit" class="search-icon"></image>
 	 -->
	
	
	
	<view class="swiper-container">
			<swiper autoplay="true" indicator-dots="ture" touchable="true">
				<swiper-item>
					<image src="../../static/lpf_home_img/scroll1.png" class="scroll1"></image>
					<div>
						<span>！！！！香蕉皮为什么看不到</span>
					</div>
				</swiper-item>
				<swiper-item>
					<image src="../../static/lpf_home_img/scroll2.png" class="scroll1"></image>
				</swiper-item>
				<swiper-item>
					<image src="../../static/lpf_home_img/scroll3.png" class="scroll1"></image>
				</swiper-item>
				<swiper-item>
					<image src="../../static/lpf_home_img/scroll4.png" class="scroll1"></image>
				</swiper-item>
			</swiper>
			
			
	</view>
	
	<view class = "chuyu" id="chuyuScr">
		<span class = "head">ss</span>
		<p>香蕉皮皮</p>
	</view>
	<!-- <view class = "chuyu" >
		<span class = "head">香蕉皮sss</span>
		<p>香蕉皮皮皮</p>
	</view> -->
	
	
	
</view>
	
	
	

	
</template>



<script>
	var chuyu = new Vue({
		el:'#chuyuScr',
		data:{
			heads:[
				{head:'香蕉皮'},
				{head:'桃子皮'},
				{head:'鸭子皮'}]
			// ],
			// infos:[
			// 	{info:'不能吃1'},
			// 	{info:'不能吃2'},
			// 	{info:'不能吃3'}
			// ]
			
		}
	})
	
	
	
	
	
	
	// export default {
	// 	data() {
	// 		return {
				
	// 		}
	// 	},
	// 	methods: {
	// 		input:function(){
	// 			uni.navigateTo({
	// 				url:"../search/search"
	// 			})
	// 		}
	// 	}
	// }
</script>

<style>
	.scroll1{
		width: 100%;
		height: 300upx;
	}
	
	.head{
		
	}
	
	.chuyu{
		border-style: solid;
		border: 1upx solid;
		
		margin: 2upx;
	}
	
	.search{
		width: input_width;
		height: 53upx;
		margin: 70upx 32upx 0upx 32upx;
		position: relative;
		background-color: #F3F4F7;
		border: 1upx solid #2EC4B6;
		font-size: 28upx;
		border-radius: 36upx;
		padding-left: 90upx;
		padding-top: 19upx;
		color: #999999;
	}
	.voice-icon{
		width: 40upx;
		height: 40upx;
		position: absolute;
		top: 86upx;
		left:62upx;
		z-index: 10;
	}
	.search-icon{
		width: 40upx;
		height: 40upx;
		position: absolute;
		top:86upx;
		right:62upx;
		z-index: 10;
	}
</style>
