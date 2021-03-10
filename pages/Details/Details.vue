<template>	
<view class="background">
	<image src="../../static/background.png" mode="aspectFit" class="background-card"></image>
	<view class="card" :style="{background: color}">
		<view class="name">	{{name}} </view>
		<view class="type">{{types[type]}}</view>
		<image v-if="!collect" src="../../static/star.png" mode="aspectFit" class="star" @click="Collect()"></image>
		<image v-if="collect" src="../../static/star_fill.png" mode="aspectFit" class="star" @click="unCollect()"></image>
		<image src="../../static/location.svg" mode="aspectFit" class="location-icon" @click="ChangeCity()"></image>
		<view class="city" @click="ChangeCity()">{{city}}</view>
	</view>
	<view class="detail">
		<view class="header">定义与处理方式</view>
		<view class="instruction">{{instruction[type]}}</view>
	</view>
	<view class="line"></view>
	<view class="share">分享至</view>
	<view class="wx-background">
		<image src="../../static/wx.png" mode="aspectFit" class="wx" @click="wx_share(name,city,types[type])"></image>
	</view>
	<view class="wx-words">微信好友</view>
	<view class="pyq-background">
		<image src="../../static/pyq.png" mode="aspectFit" class="pyq" @click="pyq_share(name,city,types[type])"></image>
	</view>
	<view class="pyq-words">微信朋友圈</view>
	<view class="about" @click="about">关于</view>
</view>
</template>

<script>
	export default {
		data() {
			return {
				types:[
				'厨余垃圾','其他垃圾','可回收垃圾','有害垃圾','非日常垃圾','装修建筑垃圾','学术垃圾',
				'易腐垃圾','其他垃圾','可回收垃圾','有害垃圾','非日常垃圾','装修建筑垃圾','学术垃圾',
				'餐厨垃圾','其他垃圾','可回收垃圾','有害垃圾','非日常垃圾','装修建筑垃圾','学术垃圾',
				'湿垃圾','干垃圾','可回收垃圾','有害垃圾','非日常垃圾','装修建筑垃圾','学术垃圾',
				],
				instruction:[
				"厨余垃圾，是指易腐的生物质生活废弃物。主要包括食材废料、剩菜剩饭、过期食品、瓜皮果核、花卉绿植、中药药渣等。",
				"其它垃圾，是指除可回收物、有害垃圾、湿垃圾以外的其它生活废弃物。",
				"可回收垃圾是指废纸张、废塑料、废玻璃制品、废金属、废织物等适宜回收、可循环利用的生活废弃物。",
				"有害垃圾是指废电池、废灯管、废药品、废油漆以及其容器等对人体健康或者自然环境造成直接或潜在危害的生物废弃物。",
				"非日常垃圾包含动物粪便等，这些垃圾不应当与生活垃圾一同丢弃，可直接倒入马桶中冲走。",
				"装修、建筑垃圾回收方式，请与相关机构或组织联系。",
				"学术垃圾，是指不努力学习，而且还天天掉头发的垃圾。这些垃圾不应当与生活垃圾一同丢弃，建议直接令其退学。",
				"易腐垃圾，是指易腐的生物质生活废弃物。主要包括食材废料、剩菜剩饭、过期食品、瓜皮果核、花卉绿植、中药药渣等。",
				"其它垃圾，是指除可回收物、有害垃圾、湿垃圾以外的其它生活废弃物。",
				"可回收垃圾是指废纸张、废塑料、废玻璃制品、废金属、废织物等适宜回收、可循环利用的生活废弃物。",
				"有害垃圾是指废电池、废灯管、废药品、废油漆以及其容器等对人体健康或者自然环境造成直接或潜在危害的生物废弃物。",
				"非日常垃圾包含动物粪便等，这些垃圾不应当与生活垃圾一同丢弃，可直接倒入马桶中冲走。",
				"装修、建筑垃圾回收方式，请与相关机构或组织联系。",
				"学术垃圾，是指不努力学习，而且还天天掉头发的垃圾。这些垃圾不应当与生活垃圾一同丢弃，建议直接令其退学。",
				"餐厨垃圾，是指易腐的生物质生活废弃物。主要包括食材废料、剩菜剩饭、过期食品、瓜皮果核、花卉绿植、中药药渣等。",
				"其它垃圾，是指除可回收物、有害垃圾、湿垃圾以外的其它生活废弃物。",
				"可回收垃圾是指废纸张、废塑料、废玻璃制品、废金属、废织物等适宜回收、可循环利用的生活废弃物。",
				"有害垃圾是指废电池、废灯管、废药品、废油漆以及其容器等对人体健康或者自然环境造成直接或潜在危害的生物废弃物。",
				"非日常垃圾包含动物粪便等，这些垃圾不应当与生活垃圾一同丢弃，可直接倒入马桶中冲走。",
				"装修、建筑垃圾回收方式，请与相关机构或组织联系。",
				"学术垃圾，是指不努力学习，而且还天天掉头发的垃圾。这些垃圾不应当与生活垃圾一同丢弃，建议直接令其退学。",
				"湿垃圾即易腐垃圾，是指易腐的生物质生活废弃物。主要包括食材废料、剩菜剩饭、过期食品、瓜皮果核、花卉绿植、中药药渣等。",
				"干垃圾即其它垃圾，是指除可回收物、有害垃圾、湿垃圾以外的其它生活废弃物。",
				"可回收垃圾是指废纸张、废塑料、废玻璃制品、废金属、废织物等适宜回收、可循环利用的生活废弃物。",
				"有害垃圾是指废电池、废灯管、废药品、废油漆以及其容器等对人体健康或者自然环境造成直接或潜在危害的生物废弃物。",
				"非日常垃圾包含动物粪便等，这些垃圾不应当与生活垃圾一同丢弃，可直接倒入马桶中冲走。",
				"装修、建筑垃圾回收方式，请与相关机构或组织联系。",
				"学术垃圾，是指不努力学习，而且还天天掉头发的垃圾。这些垃圾不应当与生活垃圾一同丢弃，建议直接令其退学。",
				],
				name:'',
				type:0,
				id:0,
				color:'#40C078',
				city:'成都',
				collect:false,
				collect_list:[],
				phonenumber:''
			}
		},
		onLoad:function(option) {
			let _this=this;
			_this.name=option.name;
			_this.type=option.type-1;
			_this.color=option.color;
			_this.id=option.id;
			uni.getStorage({
				key:"City",
				success:function(res){
					_this.city=res.data;
				},
				fail:function(){
				}
			});
			uni.getStorage({
				key:"collect_list",
				success:function(res){
					_this.collect_list=res.data.slice();
					for(let i=0;i<_this.collect_list.length;i++){
						if(_this.name==_this.collect_list[i]){
							_this.collect=true;
							break;
						}
					}
				},
			});
		},
		methods: {
			Collect:function(){
				let _this=this;
				_this.collect=true;
				uni.getStorage({
					key:'collect_list',
					success(res){
						let list = res.data;
						list.push(_this.name);
						console.log(list);
						_this.collect_list=list;
						uni.setStorage({
							key: 'collect_list',
							data: _this.collect_list
						});
					},
					fail(){
						_this.collect=[];
						_this.collect_list.push(_this.name);
						uni.setStorage({
							key: 'collect_list',
							data: _this.collect_list
						});
					}
				});
				uni.getStorage({
					key:"tel",
					success(res) {
						_this.phonenumber = res.data;
						uni.request({
							url:"https://www.shilezhihhh.cn/API/A10011",
							data:{
								data:"{\"id\":\""+item.id+"\",\"phonenumber\":\""+_this.phonenumber+"\"}"
							}
						});
					}
				});
			},
			unCollect:function(){
				let _this=this;
				_this.collect=false;
				for(let i=0;i<_this.collect_list.length;i++){
					if(_this.name==_this.collect_list[i]){
						let j=i;
						_this.collect_list.splice(j,1);
						break;
					}
				}
				uni.setStorage({
					key: 'collect_list',
					data: _this.collect_list
				});
				console.log(_this.collect_list);
			},
			ChangeCity:function(object){
				uni.reLaunch({
					url:"../Settings/Settings"
				});
			},
			wx_share:function(name,city,type){	
				uni.share({
					provider: "weixin",
					scene: "WXSceneSession",
					type: 1,
					summary: "【"+name+"】"+"在"+"【"+city+"】"+"属于"+"【"+type+"】"+"。"+"点击链接下载“绿色搜索-垃圾分类APP”:https://appstore.huawei.com/app/C100984111"+"助你垃圾分类",
					success: function (res) {
						console.log("success:" + JSON.stringify(res));
					},
					fail: function (err) {
						console.log("fail:" + JSON.stringify(err));
					}
				});
			},
			pyq_share:function(name,city,type){	
				uni.share({
					provider: "weixin",
					scene: "WXSenceTimeline",
					type: 1,
					summary: "【"+name+"】"+"在"+"【"+city+"】"+"属于"+"【"+type+"】"+"。"+"点击链接下载“绿色搜索-垃圾分类APP”:https://appstore.huawei.com/app/C100984111"+"助你垃圾分类",
					success: function (res) {
						console.log("success:" + JSON.stringify(res));
					},
					fail: function (err) {
						console.log("fail:" + JSON.stringify(err));
					}
				});			
			},
			about:function(){
				uni.navigateTo({
					url:"../about/about"
				});
			}
		}
	}
</script>

<style lang="less" scoped>
	
	.background{
		width: 750upx;
		height: 100%;
		background-color: #F5F5F5;
		position: absolute;
		top: 0upx;
		z-index: -1;
		.background-card{
			width: 670upx;
			height: 1000upx;
			position: absolute;
			right: 40upx;
			top: 86upx;
			z-index: -1;
		}
		.card{
			width: 650upx;
			height: 276upx;
			border-radius: 20upx 20upx 0upx 0upx;
			color:#FFFFFF;
			margin: 100upx 50upx;
			.name{
				font-size: 50upx;
				padding: 50upx 50upx;
			}
			.type{
				font-size: 65upx;
				padding: 0upx 48upx;
				margin: -10upx 0upx;
			}
			.star{
				width: 50upx;
				height: 50upx;
				position: absolute;
				right: 100upx;
				top: 97upx;
				z-index: 10;
			}
			.location-icon{
				width: 36upx;
				height: 36upx;
				position: absolute;
				right: 190upx;
				top: 286upx;
				z-index: 10;
			}
			.city{
				font-size: 36upx;
				position: absolute;
				right: 90upx;
				top: 280upx;
			}
		}
		.detail{
			width: 650upx;
			height: 300upx;
			margin: -99upx 50upx;
			position: relative;
			background-color: #FFFFFF;
			.header{
				color: #333333;
				font-size: 40upx;
				padding: 40upx 50upx;
			}
			.instruction{
				color: #333333;
				font-size: 35upx;
				padding: 0upx 50upx;
				margin: -20upx 0upx;
			}
		}
		.line{
			width: 550upx;
			height: 4upx;
			margin: 135upx 100upx;
			position: relative;
			background-color: #F5F5F5;
			text-align: center;
		}
		.share{
			color: #333333;
			font-size: 40upx;
			margin: -90upx 100upx;
		}
		.wx-background{
			background-color: #F7F7F7;
			width: 120upx;
			height: 120upx;
			margin: 120upx 170upx;
			border-radius: 100upx;
		}
		.wx{
			width: 60upx;
			height: 60upx;
			position: absolute;
			right: 490upx;
			top: 871upx;
		}
		.wx-words{
			color: #666666;
			font-size: 30upx;
			position: absolute;
			right: 465upx;
			top: 970upx;
		}
		.pyq-background{
			background-color: #F7F7F7;
			width: 120upx;
			height: 120upx;
			position: absolute;
			right: 180upx;
			top: 840upx;
			border-radius: 100upx;
		}
		.pyq{
			width: 60upx;
			height: 60upx;
			position: absolute;
			right: 30upx;
			top: 30upx;
		}
		.pyq-words{
			color: #666666;
			font-size: 30upx;
			position: absolute;
			right: 165upx;
			top: 970upx;
		}
		.about{
			color: #666666;
			font-size: 30upx;
			text-align: center;
		}
	}
</style>
