<template>
	<scroll-view class="swiper-tab" scroll-x :scroll-left="scrollLeft" :scroll-with-animation="true">
		<view v-for="(item, index) in qgItems" 
			:key="item.index" 
			:class="['swiper-tab-list',index == tabIndex ? 'qg_active' : '']"
			:id="index" 
			
			@tap="gettqg(item, index)" 
			>
			<view class="list-item" >
				<text>{{item.time}}</text>
				<text v-if="item.hs <= gettime && gettime <= item.he">进行中</text>
				<text v-if="gettime < item.hs">即将开始</text>
				<text v-if="gettime > item.he">已开始</text>
				<text v-if="index == tabIndex" class="qgactive-item"></text>
				<!-- <text v-if="item.hs <= gettime && gettime <= item.he" class="qgactive-item"></text> -->
			</view>
		</view>
			
		
	</scroll-view>
</template>

<script>
	export default {
		
		data() {
			return {
				tabIndex: 0,	//选中项下标
				scrollLeft:0,	//滚动位置
				qgItems:[{
					time:'0:00-8:00',
					hs:0,
					he:7
				},{
					time:'8:00-10:00',
					hs:8,
					he:9
				},{
					time:'10:00-12:00',
					hs:10,
					he:11
				},{
					time:'12:00-14:00',
					hs:12,
					he:13
				},{
					time:'14:00-16:00',
					hs:14,
					he:15
				},{
					time:'16:00-18:00',
					hs:16,
					he:17
				},{
					time:'18:00-20:00',
					hs:18,
					he:19
				},{
					time:'20:00-22:00',
					hs:20,
					he:21
				},{
					time:'22:00-24:00',
					hs:22,
					he:23
				}
				]
			}
		},
		computed: {
			gettime(){
				// 计算时间
				let state = '已开抢'
				let nowTime = new Date();
				let h =nowTime.getHours();
				
				return h;
			}
		},
		mounted(){
			console.log(this.gettime)
			for(let i in this.qgItems){
				if (this.qgItems[i].hs <= this.gettime){
					this.tabIndex = i;
					this.scrollLeft = i*50;
				}
			}
		},
		methods: {
			gettqg(item, index) {
				console.log(JSON.stringify(item))
				if (this.tabIndex !== index) {
					this.tabIndex = index;
					this.scrollLeft = index*50;
					this.$emit('clickItem', item);
				}
			}
		},
	}
</script>

<style scoped>
	.list {
		width: 750upx;
		height: 100%;
	}
	.swiper-box {
		flex: 1;
		width: 100%;
		height: calc(100% - 100upx);
	}
	.swiper-tab {
		width: 100%;
		white-space: nowrap;
		line-height: 64upx;
		height: 116upx;
		border:0;
		font-size:24upx;
	}
	.swiper-tab-list {
		font-size: 30upx;
		width: 180upx;
		padding: 5px 0;
		display: inline-block;
		text-align: center;
		color: #fff;
		background: #000;
		line-height:20px;
	}
	.list-item{
		display:flex;
		flex-direction: column;
		align-items: center;
	}
	.qg_active {
		color: #fff;
		background:#FF502E;
	}
	.qgactive-item{
		width: 0;
		height: 0;
		border-width: 5px;
		border-style: solid;
		border-color: #FF502E transparent transparent transparent;
		position:absolute;
		margin-top:45px;
	}
</style>
