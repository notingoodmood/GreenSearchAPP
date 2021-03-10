<template>
	<view>
		<view>
		
		<!-- 	<sun-tab :value.sync="swiperIndex" :tabList="tabSwiperList"></sun-tab> -->
			<swiper :current="swiperIndex" duration="300" :circular="true" indicator-color="rgba(255,255,255,0)" indicator-active-color="rgba(255,255,255,0)" @change="swiperChange">				
			<swiper-item v-for="swiper in tabSwiperList" :key="index">
					<view style="height: 1000upx;">
					
						<div class="outerBolck" v-if="isZero" style="height: 1000upx;">
							<div class="everyBlock" v-for="item in chuyu">
								<p class="everyHead">{{item.head}}</p>
								<p class="everyInfo">{{item.info}}</p>
								<image :src="item.url" class="everyImage">
							</div>
						</div>
						
						
					</view>
				</swiper-item>
			</swiper>
			
			
		</view>
	</view>
</template>

<script>
	import sunTab from '@/components/sun-tab/sun-tab.vue';
	export default {
		components: {
			sunTab
		},
		data() {
			return {
				test:'hello',
				showChuyu:true,
				chuyu:[
					{head:'香蕉皮',info:'1啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦啦',url:'../../static/banana1.jpg'},
					{head:'桃子皮',info:'2哈哈哈哈哈哈'}
				],
				
				zeroInfo:'this is zero',
				oneInfo:'this is one',
				twoInfo:'this is two',
				isZero:true,
				isOne:false,
				isTwo:false,
				index: 0,
				swiperIndex:0,
				tabList: ['选项卡一','选项卡二','选项卡三'],
				tabColorList: ['选项卡一','选项卡二','选项卡三选项卡三项卡三'],
				tabIconList: [
					{
						name: '男',
						icon: 'iconnan',
					},
					{
						name: '女',
						icon: 'iconziyuan',
					},
					{
						name: '儿童',
						icon: 'icon10',
					}
				],
				tabObjectList: [
					{
						name: '选项卡一',
						value: 1
					},
					{
						name: '选项卡二',
						value: 2
					},
					{
						name: '选项卡三',
						value: 3
					}
				],
				tabScrollIndex: 0,
				tabScrollList: ['选项卡一','选项卡二选项卡二','选项卡三','选项卡四','选项卡五选项卡五选项卡五','选项卡六','选项卡七','选项卡八'],
				tabSwiperList: ['可回收物','厨余垃圾','有害垃圾','其他垃圾'],
			}
		},
		onLoad() {

		},
		methods: {
			arrayChange(e){
				console.log('数组数据返回格式');
				console.log(e);
			},
			objectChange(e){
				console.log('对象数据返回格式');
				console.log(e);
			},
			swiperChange(e){
				this.swiperIndex = e.target.current;
				if(e.target.current === 0){
					this.isZero = true;
					this.isOne = false;
					this.isTwo = false;
				}
				if(e.target.current === 1){
					this.isZero = false;
					this.isOne = true;
					this.isTwo = false;
					
				}
				if(e.target.current === 2){
					this.isZero = false;
					this.isOne = false;
					this.isTwo = true;
				}
				
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
	// uni-page-body{
	// 	background-color: #f4f4f4;
	// }
	// .p10{
	// 	padding: 10px;
	// 	font-size: 16px;
	// }
</style>
