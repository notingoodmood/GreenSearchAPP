<template>
	<view class="about">
		<image src="../../static/logo.png" mode="aspectFit" class="logo"></image>
		<text>
			版本号：v1.0.3\n
			四川九通智路科技有限公司\n
			使用中如果遇到问题\n
			请联系：\n
			shuwei@std.uestc.edu.cn
		</text>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			
		}
	}
</script>

<style>
	.logo{
		width: 128upx;
		height: 128upx;
		padding: 18upx 40upx;
		position: absolute;
		right: 270upx;
		top: 50upx;
		z-index: 10;
	}
	.about{
		font-size: 50upx;
		color: #333333;
		margin:300upx auto;
		text-align:center;
	}
</style>
