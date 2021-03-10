<template>
<view>
	<view  class="background">
		<view v-if="show_city_setting" class="voice_setting_unshow" @click="click_city_setting()">
			语音模式
		</view>
		 <view v-if="show_city_setting" class="city_setting_show">
		 	城市设置
		 </view>
		 <view v-if="show_voice_setting" class="voice_setting_show">
		 	语音模式
		 </view>
		  <view v-if="show_voice_setting" class="city_setting_unshow" @click="click_voice_setting()">
		  	城市设置
		  </view>
	</view>
	<scroll-view scroll-y="true" style="height: 85%;position: absolute;top:180upx;">
	<view>
		 <uni-list v-if="show_voice_setting">
		 	<view>
				<uni-list-item class="list" v-for="object in Languages" style="position: relative;" :show-extra-icon=true :extra-icon=object.icon @click="ChangeAudioLanguage(object)" :title="object.name" showArrow="false"></uni-list-item>
		 	</view>
		 </uni-list>
		  <uni-list v-if="show_city_setting">
			<view>
				<uni-list-item class="list" v-for="object in Cities" style="position: relative;" :show-extra-icon=true :extra-icon=object.icon @click="ChangeCity(object)" :title="object.name" showArrow="false"></uni-list-item>
			</view>
		  </uni-list>
		   <view class="about" @click="keywordsClick()"><uni-list-item title="关于" style="position: relative;"></uni-list-item></view>
	</view>
	</scroll-view>
</view>
</template>

<script>
	import uniList from '@/components/uni-list/uni-list.vue';
	import uniListItem from '@/components/uni-list-item/uni-list-item.vue';
	export default{
		components: {uniList,uniListItem},
		data() {
			return {
				show_city_setting: true,
				show_voice_setting: false,
				Languages:[
					{
						value:"普通话",
						name:"普通话",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					},
					{
						value:"粤语",
						name:"粤\xa0\xa0\xa0\xa0语",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
						}
						,
						{
						value:"河南话",
						name:"河南话",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
						},
						{
						value:"上海话",
						name:"上海话",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
						},
						{
						value:"四川话",
						name:"四川话",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
						},
						
					],
				Cities:[
					{
						value:"成都",
						name:"成\xa0\xa0\xa0\xa0都",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"北京",
						name:"北\xa0\xa0\xa0\xa0京",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					},
					{
						value:"长春",
						name:"长\xa0\xa0\xa0\xa0春",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"重庆",
						name:"重\xa0\xa0\xa0\xa0庆",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"长沙",
						name:"长\xa0\xa0\xa0\xa0沙",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"大连",
						name:"大\xa0\xa0\xa0\xa0连",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"德阳",
						name:"德\xa0\xa0\xa0\xa0阳",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"福州",
						name:"福\xa0\xa0\xa0\xa0州",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"广元",
						name:"广\xa0\xa0\xa0\xa0元",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"贵阳",
						name:"贵\xa0\xa0\xa0\xa0阳",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"广州",
						name:"广\xa0\xa0\xa0\xa0州",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"邯郸",
						name:"邯\xa0\xa0\xa0\xa0郸",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"哈尔滨",
						name:"哈尔滨",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"合肥",
						name:"合\xa0\xa0\xa0\xa0肥",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"海口",
						name:"海\xa0\xa0\xa0\xa0口",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"杭州",
						name:"杭\xa0\xa0\xa0\xa0州",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"济南",
						name:"济\xa0\xa0\xa0\xa0南",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"昆明",
						name:"昆\xa0\xa0\xa0\xa0明",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"拉萨",
						name:"拉\xa0\xa0\xa0\xa0萨",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"兰州",
						name:"兰\xa0\xa0\xa0\xa0州",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"宁波",
						name:"宁\xa0\xa0\xa0\xa0波",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"南京",
						name:"南\xa0\xa0\xa0\xa0京",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"南宁",
						name:"南\xa0\xa0\xa0\xa0宁",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"青岛",
						name:"青\xa0\xa0\xa0\xa0岛",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"上海",
						name:"上\xa0\xa0\xa0\xa0海",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"石家庄",
						name:"石家庄",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"沈阳",
						name:"沈\xa0\xa0\xa0\xa0阳",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"深圳",
						name:"深\xa0\xa0\xa0\xa0圳",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"苏州",
						name:"苏\xa0\xa0\xa0\xa0州",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"泰安",
						name:"泰\xa0\xa0\xa0\xa0安",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"铜陵",
						name:"铜\xa0\xa0\xa0\xa0陵",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"太原",
						name:"太\xa0\xa0\xa0\xa0原",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"西安",
						name:"西\xa0\xa0\xa0\xa0安",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"厦门",
						name:"厦\xa0\xa0\xa0\xa0门",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"咸阳",
						name:"咸\xa0\xa0\xa0\xa0阳",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"宜春",
						name:"宜\xa0\xa0\xa0\xa0春",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"银川",
						name:"银\xa0\xa0\xa0\xa0川",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					,
					{
						value:"宜昌",
						name:"宜\xa0\xa0\xa0\xa0昌",
						icon:{color:'#2EC4B6',size:'22',type:'circle'}
					}
					],
		}
		},
		onShow:function(){
			let _this=this;
			uni.getStorage({
				key:"AudioLanguage",
				success:function(res){
					for(var i=0;i<_this.Languages.length;i++){
						if(_this.Languages[i].value==res.data){
							_this.Languages[i].icon={color:'#2EC4B6',size:'22',type:'circle-filled'};
						}else{
							_this.Languages[i].icon={color:'#2EC4B6',size:'22',type:'circle'};
						}
					}
				},
				fail:function(){
					_this.Languages[0].icon={color:'#2EC4B6',size:'22',type:'circle-filled'};
				}
			});
			uni.getStorage({
				key:"City",
				success:function(res){
					for(var i=0;i<_this.Cities.length;i++){
						if(_this.Cities[i].value==res.data){
							_this.Cities[i].icon={color:'#2EC4B6',size:'22',type:'circle-filled'};
						}else{
							_this.Cities[i].icon={color:'#2EC4B6',size:'22',type:'circle'};
						}
					}
				},
				fail:function(){
					_this.Cities[0].icon={color:'#2EC4B6',size:'22',type:'circle-filled'};
				}
			});
		},
		methods: {
			click_city_setting:function(){
				let _this = this;
				_this.show_city_setting = false;
				_this.show_voice_setting = true;
			},
			click_voice_setting:function(){
				let _this = this;
				_this.show_city_setting = true;
				_this.show_voice_setting = false;
			},
			keywordsClick:function(object){
				uni.navigateTo({
					url:"../about/about"
				});
			},
			ChangeAudioLanguage:function(object){
				let _this=this;
				for(var i=0;i<_this.Languages.length;i++){
					if(_this.Languages[i].value.includes(object.value)){
						_this.Languages[i].icon={color:'#2EC4B6',size:'22',type:'circle-filled'};
					}else{
						_this.Languages[i].icon={color:'#2EC4B6',size:'22',type:'circle'};
					}
				}
				uni.setStorage({
					key:"AudioLanguage",
					data:object.value
				});
			},
			ChangeCity:function(object){
				let _this=this;
				for(var i=0;i<_this.Cities.length;i++){
					if(_this.Cities[i].value.includes(object.value)){
						_this.Cities[i].icon={color:'#2EC4B6',size:'22',type:'circle-filled'};
					}else{
						_this.Cities[i].icon={color:'#2EC4B6',size:'22',type:'circle'};
					}
				}
				uni.setStorage({
					key:"City",
					data:object.value
				});
			}
		}
	}
</script>

<style lang="less" scoped>
	.background{
		height: 15%;
	}
	.city_setting_show{
		font-size: 32upx;
		color: #FFFFFF;
		width: 180upx;
		height: 48upx;
		position: absolute;
		top:70upx;
		left:141upx;
		background-color: #2EC4B6;
		border-radius: 100upx 0upx 0upx 100upx;
		padding: 30upx;
		text-align: center;
	}
	.voice_setting_unshow{
		font-size: 32upx;
		width: 180upx;
		height: 45upx;
		position: absolute;
		top:70upx;
		left:379upx;
		color: #2EC4B6;
		background-color: #FFFFFF;
		border-radius: 0upx 100upx 100upx 0upx;
		padding: 30upx;
		text-align: center;
		border: 1upx solid #2EC4B6;
	}
	.city_setting_unshow{
		font-size: 32upx;
		width: 180upx;
		height: 45upx;
		position: absolute;
		top:70upx;
		left:141upx;
		color: #2EC4B6;
		background-color: #FFFFFF;
		border-radius: 100upx 0upx 0upx 100upx;
		padding: 30upx;
		text-align: center;
		border: 1upx solid #2EC4B6;
	}
	.voice_setting_show{
		font-size: 32upx;
		width: 180upx;
		height: 48upx;
		position: absolute;
		top:70upx;
		left:379upx;
		color: #FFFFFF;
		background-color: #2EC4B6;
		border-radius: 0upx 100upx 100upx 0upx;
		padding: 30upx;
		text-align: center;
	}
	.list{
		display: inline-flex;
		flex-wrap: wrap;
		padding: 0upx 25upx 20upx 90upx;
		border-radius: 60upx;
	}
	.about{
		font-size: 35upx;
		margin:80upx 220upx;
		border-radius: 60upx;
	}
</style>
