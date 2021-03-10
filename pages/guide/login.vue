<template>
	<view>
		<image src="../../static/logo.png" class="logo"></image>
		<view v-if="!show_input" class="pass" @click="pass">跳过</view>
		<view v-if="show_input" class="small_pass" @click="pass">跳过</view>
		<view>
			<input class="phonenumber" maxlength="20" focus type="text" confirm-type="next" placeholder="输入手机号" value="" v-model.trim="phonenumber"/>
			<view v-if="!show_input" class="send" @click="send">发送验证码</view>
			<input v-if="show_input" class="psw" maxlength="20" focus type="text" confirm-type="next" placeholder="输入验证码" value="" v-model.trim="psw" />
			<view class="resend" v-if="show_input" @click="send">
				<image class="resend_icon" src="../../static/resend.png"></image>
			</view>
			<view class="login" v-if="show_input" @click="login">
				<image class="login_icon" src="../../static/login.png"></image>
			</view>
		</view>
		<view class="about" @click="about">关于</view>
	</view>
</template>

<script>
	export default{
		data(){
			return {
				show_input:false,
				show_time:false,
				phonenumber:'',
				psw:''
			}
		},
		methods: {
			send:function(){
				let _this=this;
				var reg =/^0?1[3|4|5|6|7|8][0-9]\d{8}$/;//正则表达式，用于判断手机号
				if(_this.phonenumber=="AAAAAAAAAAA"){
					_this.show_input=true;
					uni.showToast({
						title: '测试账号，不会发送短信，直接在验证码框输入密码',
						icon: 'none',
						duration: 2500
					});
				}
				else if(!reg.test(_this.phonenumber)){
					uni.showToast({
						title: '请输入正确的手机号',
						icon: 'none',
						duration: 1000
					});
				}
				else{
					//发送验证码
					uni.request({
						url:"https://www.shilezhihhh.cn/API/A10002",
						data:{
							data:"{\"phonenumber\":\""+_this.phonenumber+"\"}"
						},
						dataType:"text",
						responseType:"text",
						success:function(res){
							let obj=JSON.parse(res.data);
							if(obj.errorcode==0){
								uni.showToast({
									title: '验证码已发送',
									icon: 'none',
									duration: 1000
								});
								_this.show_input=true;
							}
							else if(obj.errorcode==1){
								uni.showToast({
									title: '请稍后再试',
									icon: 'none',
									duration: 1000
								});
							}
							else if(obj.errorcode==2){
								uni.showToast({
									title: '今日发送次数已达上限',
									icon: 'none',
									duration: 1000
								});
							}
							else{
								uni.showToast({
									title: '请重试',
									icon: 'none',
									duration: 1000
								});
							}
						},
						fail() {
							uni.showToast({
								title: '请重试',
								icon: 'none',
								duration: 1000
							});
						}
					}); 
				}
			},
			about:function(){
				uni.navigateTo({
					url:"../about/about"
				});
			},
			pass:function(){
				uni.switchTab({
					url:"../adbdjbajdbkakdadcc/adbdjbajdbkakdadcc"
				});
			},
			login:function(){
				let _this=this;
				var reg =/^0?1[3|4|5|6|7|8][0-9]\d{8}$/;//正则表达式，用于判断手机号
				if(!reg.test(_this.phonenumber) && _this.phonenumber!="AAAAAAAAAAA"){
					uni.showToast({
						title: '请输入正确的手机号',
						icon: 'none',
						duration: 1000
					});
				}
				else if(_this.psw.length==0){
					uni.showToast({
						title: '请输入验证码',
						icon: 'none',
						duration: 1000
					});
				}
				uni.request({
					url:"https://www.shilezhihhh.cn/API/A10003",
					data:{
						data:"{\"phonenumber\":\""+_this.phonenumber+"\",\"code\":\""+_this.psw+"\"}"
					},
					dataType:"text",
					responseType:"text",
					success:function(res){
						let obj=JSON.parse(res.data);
						if(obj.errorcode==0){
							uni.showToast({
								title: '登陆成功',
								icon: 'none',
								duration: 1000
							});
							uni.setStorage({//登陆成功之后，存token
								key:"token",
								data:obj.token
							});
							uni.setStorage({
								key:"tel",
								data:_this.phonenumber
							});
							uni.navigateTo({
								url:"./ChooseCity"
							});
						}
						else if(obj.errorcode==1){
							uni.showToast({
								title: '验证码已过期',
								icon: 'none',
								duration: 1000
							});
						}
						else if(obj.errorcode==2){
							uni.showToast({
								title: '验证码错误',
								icon: 'none',
								duration: 1000
							});
						}
						else if(obj.errorcode==3){
							uni.showToast({
								title: '请重新输入手机号',
								icon: 'none',
								duration: 1000
							});
							_this.show_input=false;
						}
						else{
							uni.showToast({
								title: '请重试',
								icon: 'none',
								duration: 1000
							});
							_this.show_input=false;
						}
					},
					fail() {
						uni.showToast({
							title: '请重试',
							icon: 'none',
							duration: 1000
						});
						_this.show_input=false;
					}
				});
			}
		}
	}
</script>

<style>
	.logo{
		width: 128upx;
		height: 128upx;
		position: absolute;
		top:280upx;
		left:311upx;
		z-index: 10;
	}
	.pass{
		background-color: #F3F4F8;
		color: #666666;
		width: 200upx;
		height: 55upx;
		font-size: 40upx;
		text-align: center;
		padding: 25upx;
		position: absolute;
		top: 760upx;
		left: 255upx;
		border-radius: 60upx;
		border: 1upx solid #CCCCCC;
	}
	.small_pass{
		background-color: #F3F4F8;
		color: #666666;
		width: 100upx;
		height: 55upx;
		font-size: 40upx;
		text-align: center;
		padding: 10upx;
		position: absolute;
		top: 65upx;
		right: 25upx;
		border-radius: 60upx;
		border: 1upx solid #CCCCCC;
	}
	.phonenumber{
			background-color: #F3F4F8;
			height: 70upx;
			margin: 470upx 90upx 0upx 90upx;
			text-align: center;
			padding: 22upx 30upx;
			font-size: 40upx;
			border-radius: 60upx;
			caret-color: #2EC4B6;
			border: 1upx solid #2EC4B6;
	}
	.send{
		color: #FFFFFF;
		background-color: #2EC4B6;
		height: 55upx;
		margin: 25upx 90upx 0upx 90upx;
		padding: 25upx;
		text-align: center;
		font-size: 40upx;
		border-radius: 60upx;
		caret-color: #2EC4B6;
	}
	.psw{
		background-color: #F3F4F8;
		height: 70upx;
		margin: 25upx 210upx 0upx 90upx;
		text-align: center;
		padding: 22upx 30upx;
		font-size: 40upx;
		border-radius: 60upx;
		caret-color: #2EC4B6;
		border: 1upx solid #2EC4B6;
	}
	.resend{
		font-size: 60upx;
		text-align: center;
		color: #FFFFFF;
		background-color: #2EC4B6;
		border-radius: 100upx;
		height: 100upx;
		width: 100upx;
		position: absolute;
		top: 610upx;
		right: 90upx;
	}
	.resend_icon{
		padding: 22upx;
		height: 60upx;
		width: 60upx;
	}
	.login{
		font-size: 60upx;
		text-align: center;
		color: #FFFFFF;
		background-color: #0093E7;
		border-radius: 100upx;
		height: 160upx;
		width: 160upx;
		position: absolute;
		top: 770upx;
		right: 290upx;
	}
	.login_icon{
		padding: 50upx;
		height: 60upx;
		width: 60upx;
	}
	.about{
		font-size: 35upx;
		margin:70% auto 10upx auto;
		border-radius: 60upx;
		text-align: center;
		color: #666666
	}
</style>
