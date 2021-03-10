<template>
	<view>
		<view class="uni-padding-wrap uni-common-mt">
		            <view class="uni-title uni-common-mt">
		                Vertical Scroll
		                <text>\n纵向滚动</text>
		            </view>
		            <view>
		                <scroll-view :scroll-top="scrollTop" scroll-y="true" class="scroll-Y" @scrolltoupper="upper" @scrolltolower="lower"
		                @scroll="scroll">
		                    <view id="demo1" class="scroll-view-item uni-bg-red">A</view>
		                    <view id="demo2" class="scroll-view-item uni-bg-green">B</view>
		                    <view id="demo3" class="scroll-view-item uni-bg-blue">C</view>
		                </scroll-view>
		            </view>
		            <view @tap="goTop" class="uni-link uni-center uni-common-mt">
		                点击这里返回顶部
		            </view>
		            <view class="uni-title uni-common-mt">
		                Horizontal Scroll
		                <text>\n横向滚动</text>
		            </view>
		            <view>
		                <scroll-view class="scroll-view_H" scroll-x="true" @scroll="scroll" scroll-left="120">
		                    <view id="demo1" class="scroll-view-item_H uni-bg-red">A</view>
		                    <view id="demo2" class="scroll-view-item_H uni-bg-green">B</view>
		                    <view id="demo3" class="scroll-view-item_H uni-bg-blue">C</view>
		                </scroll-view>
		            </view>
		        </view>
			
			
		<div class="choose">
		<span @click="chooseAction()" class="chooseTrash">厨余垃圾</span>
		<span @click="chooseAction()" class="chooseTrash">可回收物</span>
		<span @click="chooseAction()" class="chooseTrash">其他垃圾</span>
		<span @click="chooseAction()" class="chooseTrash">有害垃圾</span>
		</div>
		<swiper>
			<swiper-item>
				<image src="../../static/lpf_home_img/scroll1.png" class="swiper_every"></image>
			</swiper-item>
			
			<swiper-item>
				<image src="../../static/lpf_home_img/scroll2.png" class="swiper_every"></image>
			</swiper-item>
			
			<swiper-item>
				<image src="../../static/lpf_home_img/scroll3.png" class="swiper_every"></image>
			</swiper-item>
			
			<swiper-item>
				<image src="../../static/lpf_home_img/scroll4.png" class="swiper_every"></image>
			</swiper-item>
		</swiper>
		
		<div class="outerBolck">
			<div class="everyBlock" v-for="item in chuyu">
				<p class="everyHead">{{item.head}}</p>
				<p class="everyInfo">{{item.info}}</p>
				<image :src="item.url" class="everyImage">
			</div>
		</div>
	</view>
	
	
	
	
	
	
	
	
</template>

<script>
	import '../../common/uni.css';
	import './uni-title.vue'
	
	export default {
		
		data() {
			return {
				test:'hello',
				showChuyu:true,
				chuyu:[
					{head:'香蕉皮',info:'1啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦',url:'../../static/banana1.jpg'},
					{head:'桃子皮',info:'2哈哈哈哈哈哈'}
				],
				scrollTop: 0,
				old: {
					scrollTop: 0,
				}
				
			}
		},
		methods: {
			chooseAction(event){
				console.log(event)
			},
			upper: function(e) {
			            console.log(e)
			        },
			        lower: function(e) {
			            console.log(e)
			        },
			        scroll: function(e) {
			            console.log(e)
			            this.old.scrollTop = e.detail.scrollTop
			        },
			        goTop: function(e) {
			            this.scrollTop = this.old.scrollTop
			            this.$nextTick(function() {
			                this.scrollTop = 0
			            });
			            uni.showToast({
			                icon:"none",
			                title:"纵向滚动 scrollTop 值已被修改为 0"
			            })
			        }
			
			
			
		}
	}
</script>

<style>
	.chooseTrash{
		margin-left: 20upx;
	}
	
	
	
	
	
	//所有框的整体
	.outerBolck{
		margin-top: 40upx;
	}
	.everyBlock{
		position: relative;
		border-style: solid;
		border-width: 1upx;
		border-color: #C8C7CC;
		border-color: #F1F1F1;
			
	}
	.everyHead{
		margin-left: 3%;
		font-size: 50upx;
		
	}
	.everyInfo{
		text-indent: 40upx;
		margin-right: 180upx;
		margin-left: 10upx;
		font-size: 34upx;
	}
	.swiper_every{
		
		width: 100%;
	}
	.everyImage{
		margin-left: 78%;
		margin-top: 6%;
		top: 0;
		position: absolute;
		height: 135upx;
		width: 135upx;
	}

</style>
