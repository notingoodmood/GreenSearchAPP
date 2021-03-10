<template>
	<view>
		<!-- 头部便民小站字样，还未得到UI -->
		<!-- 目前的“便民小站”写在pages.json中 -->
		<!-- <view class="page-title">
			<image class="top" src="/static/News_off.png"></image>
		</view> -->
		
		<!-- 返回的商品list中，每个item有四项：pic_id,pic_link,name,link -->
		<!-- 调用uni-card显示列表中的所有商品 -->
		<view class="show-goods">
			<view v-for= "item in list" :key="item.pic_id" class = "goodings">
				<uni-card :is-shadow = "true"  mode="style" :thumbnail = "item.pic_link" @click="clickCard(item.link)">
					<view>
						{{item.name}}
					</view>
				</uni-card>
			</view>
		</view>
	</view>
</template>
<script>
	import uniCard from '@/components/uni-card/uni-card.vue'
	export default {
		components: {
			uniCard
		},
		data() {
			return{
				//返回商品列表，合并了random和interest
				list: []	
			}
		},
		onLoad() {
			//请求后端数据
			let _this = this;
			uni.request({
				dataType:"json",
				url:"https://www.shilezhihhh.cn/API/A10012",
				//这里需要用户的电话号码，现在的电话号码是固定的用来测试
				data:{
					data:"{\"phonenumber\":\"12345678900\"}"
				},
				method:"GET",
				success:function(res){
					console.log("success");
					let ran = res.data.random;
					let interest = res.data.interest;
					let list = ran.concat(interest);
					_this.list = list;
				}
			});
		},
		methods: {
			// 点击图片,跳转到商品链接
			clickCard(link) {
				plus.runtime.openURL(link);
			},
		},
	}
</script>

<style>
	page {
		display: flex;
		flex-direction: column;
		box-sizing: border-box;
		background-color:#F8F8F8
	}
	view {
		font-size: 28upx;
		line-height: inherit
	}
	.page-title{
		height :10upx;
		width:375upx;
		position:fixed; 
		top:0;
		left:0;
	}
	.show-goods{
		padding: 30upx;
		/* background:#4CD964 */
		/* position:fixed; */
		top:100upx;
		left:0;
	}
	.goodings{
		length: 200upx;
		width: 340upx;
		float:left;
		
		margin-bottom: 30upx;
	}
</style>