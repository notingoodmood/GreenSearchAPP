<template>
<view>
		<view class="top" v-if="have1">
		<uni-list>
			<uni-list-item class="item" v-for="object in ExactResult" v-bind:style="{color: object.color}" show-extra-icon="true" :extra-icon=object.icon :title=object.name @click="keywordsClick(object)" :note=types[object.type-1] show-arrow="false"></uni-list-item>
		</uni-List>
		</view>
		<view class="top" v-if="have2">
			<view class="header"> 与[{{keyword}}]有关的搜索结果：</view>
		<uni-list>
			<uni-list-item class="item" v-for="object in LianxiangResult" v-bind:style="{color: object.color}" show-extra-icon="true" :extra-icon=object.icon :title=object.name @click="keywordsClick(object)" :note=types[object.type-1] show-arrow="false"></uni-list-item>
		</uni-list>
		</view>
		<view class="top" v-if="have3">
			<view class="header" v-if="!have2 && have3"> 与[{{keyword}}]有关的搜索结果：</view>
		<uni-list>
			<uni-list-item class="item" v-for="object in YanshengwuResult" v-bind:style="{color: object.color}" show-extra-icon="true" :extra-icon=object.icon :title=object.name @click="keywordsClick(object)" :note=types[object.type-1] show-arrow="false"></uni-list-item>
		</uni-list>
		</view>
		<view class="top" v-if="have4">
			<view class="header"> 或许你想搜的是...</view>
		<uni-list>
			<uni-list-item class="item" v-for="object in QuanpinResult" v-bind:style="{color: object.color}" show-extra-icon="true" :extra-icon=object.icon :title=object.name @click="keywordsClick(object)" :note=types[object.type-1] show-arrow="false"></uni-list-item>
		</uni-list>
		</view>
		<view class="other" v-if="nohave">
		<text class='inner-box'>
         少侠好武功！ \n
        竟然发现了我们都不知道的\n
		新品种垃圾！
		</text>
		</view>
	</view>
</template>

<script>
	import uniList from '@/components/uni-list/uni-list.vue';
	import uniListItem from '@/components/uni-list-item/uni-list-item.vue';
	export default {
		components: {uniList,uniListItem},
		data() {
			return {
				keyword:'',
				ExactResult:[],//精确结果
				LianxiangResult:[],//联想搜索结果
				QuanpinResult:[],//全拼搜索结果
				FamilyName:'',//衍生物系列类别名
				YanshengwuResult:[],//衍生物系列搜索结果
				have1:false,
				have2:false,
				have3:false,
				have4:false,
				nohave:false,
				types:[],
				types1:['厨余垃圾','其他垃圾','可回收垃圾','有害垃圾','非日常垃圾','装修、建筑垃圾','学术垃圾'],
				types2:['易腐垃圾','其他垃圾','可回收垃圾','有害垃圾','非日常垃圾','装修、建筑垃圾','学术垃圾'],
				types3:['餐厨垃圾','其他垃圾','可回收垃圾','有害垃圾','非日常垃圾','装修、建筑垃圾','学术垃圾'],
				types4:['湿垃圾','干垃圾','可回收垃圾','有害垃圾','非日常垃圾','装修、建筑垃圾','学术垃圾'],
				cityCode:14,
				type:0
			}
		},
		onLoad:function(option){
			let _this=this;
			_this.keyword=option.keyword;
			_this.FamilyName='';
			_this.ExactResult=[];
			_this.QuanpinResult=[];
			_this.LianxiangResult=[];
			_this.YanshengwuResult=[];
			var localcity="成都";
			uni.getStorage({
				key:"City",
				success:function(res){
					localcity=res.data;
					console.log(res.data);
				},
				fail:function(){
				}
			});
			switch (localcity){
				case "北京":
				case "哈尔滨":
				case "宁波":
				case "西安":
				case "厦门":
				case "西宁":
				case "宜春":
				case "银川":
				case "郑州":
				{	for(var index=0;index<_this.types1.length;index++){
						_this.types.push(_this.types1[index]);
					}
					_this.cityCode=0;
					break;
				}
				case "长春":
				case "重庆":
				case "长沙":
				case "大连":
				case "德阳":
				case "福州":
				case "贵阳":
				case "合肥":
				case "海口":
				case "昆明":
				case "兰州":
				case "南宁":
				case "石家庄":
				case "苏州":
				case "铜陵":
				case "太原":
				case "芜湖":
				case "咸阳":
				{	for(var index=0;index<_this.types2.length;index++){
						_this.types.push(_this.types2[index]);
					}
					_this.cityCode=7;
					break;
				}
				case "成都":
				case "广元":
				case "广州":
				case "杭州":
				case "济南":
				case "拉萨":
				case "南京":
				case "青岛":
				case "沈阳":
				case "深圳":
				case "泰安":
				case "宜昌":
				{	for(var index=0;index<_this.types3.length;index++){
						_this.types.push(_this.types3[index]);
					}
					_this.cityCode=14;
					break;
				}
				case "邯郸":
				case "上海":
				{	for(var index=0;index<_this.types4.length;index++){
						_this.types.push(_this.types4[index]);
					}
					_this.cityCode=21;
					break;
				}
				default:
				{	for(var index=0;index<_this.types2.length;index++){
						_this.types.push(_this.types2[index]);
					}
					_this.cityCode=7;
					break;
				}
			}
			uni.request({
				url:"https://www.shilezhihhh.cn/API/A10006",
				dataType:"text",
				responseType:"text",
				data:{
					data:"{\"input\":\""+_this.keyword+"\"}"
				},
				success:function(res){
				let obj=JSON.parse(res.data);
				_this.FamilyName=obj.familyname;
				for(var index=0;index<obj.jingque.length;index++){
					_this.ExactResult.push(obj.jingque[index]);
					switch(obj.jingque[index].type){
						case 1: 
							if(_this.cityCode==21){
								_this.ExactResult[index].color='#835448';
								_this.ExactResult[index].icon={color: '#835448',size: '22',type: 'info-filled'};
							}
							else{
								_this.ExactResult[index].color='#40C078';
								_this.ExactResult[index].icon={color: '#40C078',size: '22',type: 'info-filled'};
							}
							break;
						case 2:
							if(_this.cityCode==21){
								_this.ExactResult[index].color='#5D5D5D';
								_this.ExactResult[index].icon={color: '#5D5D5D',size: '22',type: 'info-filled'};
							}
							else{
								_this.ExactResult[index].color='#EAA111';
								_this.ExactResult[index].icon={color: '#EAA111',size: '22',type: 'info-filled'};
							}
							break;
						case 3:
							_this.ExactResult[index].color='#0093E7';
							_this.ExactResult[index].icon={color: '#0093E7',size: '22',type: 'info-filled'};
							break;
						case 4:
							_this.ExactResult[index].color='#E53154';
							_this.ExactResult[index].icon={color: '#E53154',size: '22',type: 'info-filled'};
							break;
						case 5:
							_this.ExactResult[index].color='#333333';
							_this.ExactResult[index].icon={color: '#333333',size: '22',type: 'info-filled'};
							break;
						case 6:
							_this.ExactResult[index].color='#333333';
							_this.ExactResult[index].icon={color: '#333333',size: '22',type: 'info-filled'};
							break;
						case 7:
							_this.ExactResult[index].color='#696969';
							_this.ExactResult[index].icon={color: '#696969',size: '22',type: 'info-filled'};
							break;
					}
					_this.have1=true;
				}
				for(var index=0;index<obj.lianxiang.length;index++){
					_this.LianxiangResult.push(obj.lianxiang[index]);
					switch(obj.lianxiang[index].type){
						case 1: 
							if(_this.cityCode==21){
								_this.LianxiangResult[index].color='#835448';
								_this.LianxiangResult[index].icon={color: '#835448',size: '22',type: 'info-filled'};
							}
							else{
								_this.LianxiangResult[index].color='#40C078';
								_this.LianxiangResult[index].icon={color: '#40C078',size: '22',type: 'info-filled'};
							}
							break;
						case 2:
							if(_this.cityCode==21){
								_this.LianxiangResult[index].color='#5D5D5D';
								_this.LianxiangResult[index].icon={color: '#5D5D5D',size: '22',type: 'info-filled'};
							}
							else{
								_this.LianxiangResult[index].color='#EAA111';
								_this.LianxiangResult[index].icon={color: '#EAA111',size: '22',type: 'info-filled'};
							}
							break;
						case 3:
							_this.LianxiangResult[index].color='#0093E7';
							_this.LianxiangResult[index].icon={color: '#0093E7',size: '22',type: 'info-filled'};
							break;
						case 4:
							_this.LianxiangResult[index].color='#E53154';
							_this.LianxiangResult[index].icon={color: '#E53154',size: '22',type: 'info-filled'};
							break;
						case 5:
							_this.LianxiangResult[index].color='#333333';
							_this.LianxiangResult[index].icon={color: '#333333',size: '22',type: 'info-filled'};
							break;
						case 6:
							_this.LianxiangResult[index].color='#333333';
							_this.LianxiangResult[index].icon={color: '#333333',size: '22',type: 'info-filled'};
							break;
						case 7:
							_this.LianxiangResult[index].color='#696969';
							_this.LianxiangResult[index].icon={color: '#696969',size: '22',type: 'info-filled'};
							break;
					}
					_this.have2=true;
				}
				for(var index=0;index<obj.yanshengwu.length;index++){
					_this.YanshengwuResult.push(obj.yanshengwu[index]);
					switch(obj.yanshengwu[index].type){
						case 1: 
							if(_this.cityCode==21){
								_this.YanshengwuResult[index].color='#835448';
								_this.YanshengwuResult[index].icon={color: '#835448',size: '22',type: 'info-filled'};
							}
							else{
								_this.YanshengwuResult[index].color='#40C078';
								_this.YanshengwuResult[index].icon={color: '#40C078',size: '22',type: 'info-filled'};
							}
							break;
						case 2:
							if(_this.cityCode==21){
								_this.YanshengwuResult[index].color='#5D5D5D';
								_this.YanshengwuResult[index].icon={color: '#5D5D5D',size: '22',type: 'info-filled'};
							}
							else{
								_this.YanshengwuResult[index].color='#EAA111';
								_this.YanshengwuResult[index].icon={color: '#EAA111',size: '22',type: 'info-filled'};
							}
							break;
						case 3:
							_this.YanshengwuResult[index].color='#0093E7';
							_this.YanshengwuResult[index].icon={color: '#0093E7',size: '22',type: 'info-filled'};
							break;
						case 4:
							_this.YanshengwuResult[index].color='#E53154';
							_this.YanshengwuResult[index].icon={color: '#E53154',size: '22',type: 'info-filled'};
							break;
						case 5:
							_this.YanshengwuResult[index].color='#333333';
							_this.YanshengwuResult[index].icon={color: '#333333',size: '22',type: 'info-filled'};
							break;
						case 6:
							_this.YanshengwuResult[index].color='#333333';
							_this.YanshengwuResult[index].icon={color: '#333333',size: '22',type: 'info-filled'};
							break;
						case 7:
							_this.YanshengwuResult[index].color='#696969';
							_this.YanshengwuResult[index].icon={color: '#696969',size: '22',type: 'info-filled'};
							break;
					}
					_this.have3=true;
				}
				for(var index=0;index<obj.quanpin.length;index++){
					_this.QuanpinResult.push(obj.quanpin[index]);
					switch(obj.quanpin[index].type){
						case 1: 
							if(_this.cityCode==21){
								_this.QuanpinResult[index].color='#835448';
								_this.QuanpinResult[index].icon={color: '#835448',size: '22',type: 'info-filled'};
							}
							else{
								_this.QuanpinResult[index].color='#40C078';
								_this.QuanpinResult[index].icon={color: '#40C078',size: '22',type: 'info-filled'};
							}
							break;
						case 2:
							if(_this.cityCode==21){
								_this.QuanpinResult[index].color='#5D5D5D';
								_this.QuanpinResult[index].icon={color: '#5D5D5D',size: '22',type: 'info-filled'};
							}
							else{
								_this.QuanpinResult[index].color='#EAA111';
								_this.QuanpinResult[index].icon={color: '#EAA111',size: '22',type: 'info-filled'};
							}
							break;
						case 3:
							_this.QuanpinResult[index].color='#0093E7';
							_this.QuanpinResult[index].icon={color: '#0093E7',size: '22',type: 'info-filled'};
							break;
						case 4:
							_this.QuanpinResult[index].color='#E53154';
							_this.QuanpinResult[index].icon={color: '#E53154',size: '22',type: 'info-filled'};
							break;
						case 5:
							_this.QuanpinResult[index].color='#333333';
							_this.QuanpinResult[index].icon={color: '#333333',size: '22',type: 'info-filled'};
							break;
						case 6:
							_this.QuanpinResult[index].color='#333333';
							_this.QuanpinResult[index].icon={color: '#333333',size: '22',type: 'info-filled'};
							break;
						case 7:
							_this.QuanpinResult[index].color='#696969';
							_this.QuanpinResult[index].icon={color: '#696969',size: '22',type: 'info-filled'};
							break;
					}
					_this.have4=true;
				}
				if(!_this.have1 && !_this.have2 && !_this.have3 && !_this.have4){
					_this.nohave=true;
				}
			}
			});	
		},
		methods: {
			keywordsClick:function(object){
				let _this=this;
				_this.name=object.name;
				_this.type=object.type+_this.cityCode;
				_this.color=object.color;
				uni.request({
					dataType:"text",
					responseType:"text",
					url:"https://www.shilezhihhh.cn/API/A10007",
					data:{
						data:"{\"id\":\""+object.id+"\"}"
					},
				});
				uni.navigateTo({
				 	url:"../Details/Details?name="+_this.name+"&type="+_this.type+"&color="+_this.color+"&id="+object.id
				});
				
			}
		}
	}
</script>

<style>
	.header{
		padding: 30upx 0upx 0upx 32upx;
		color: #333333;
		font-size: 45upx;
	}
	.item{
		font-size: 32upx;
		color: #333333;
	}
	.other{
		font-size: 50upx;
		margin:300upx auto;
		text-align:center;
	}
</style>
