<template name="zy-search">
	<view>
		<view class="search">
			<!-- #ifdef APP-PLUS -->
				<image src="../../static/yuyin.svg" mode="aspectFit" @click="startRecognize()" class="voice-icon"></image>
				<image src="../../static/image.png" mode="aspectFit" @click="StartToSearchWithImage()" class="image-icon"></image>
			<!-- #endif -->
			<input maxlength="20" type="text" value="" focus="true" confirm-type="search" @focus="HideEveryThing" @input="inputCallback" @confirm="Search()" placeholder="请输入关键词搜索" v-model.trim="searchText"/>
			<image v-if="searchText.length>0" src="../../static/shanchu.svg" mode="aspectFit" @click="CleanUpWhenBackListener()" class="delete-icon"></image>
			<image v-if="searchText.length<=0" src="../../static/sousuo.svg" mode="aspectFit" @click="Search()" class="search-icon"></image>
		</view>
		<view v-if="ShowList||ShowWords" @click="cancel" class="cancel">
			取消
		</view>
		<view v-if="ShowWords" style="padding: 20upx 30upx 0upx 0upx;color: #333333;">
			<view style="color: #2EC4B6;font-size: 40upx;padding: 20upx 32upx;" @click="Search()">搜索:  {{searchText}} </view>
			<view style="background-color: #E5E5E5; height: 1upx; width: 686upx;margin: 0upx 32upx;"></view>
			<uni-list>
				<uni-list-item v-for="(object,i) in ExactResult" v-bind:key="i" style="position: relative;"  :title=object.name @click="keywordsClick(object)" show-arrow="false"></uni-list-item>
			</uni-List>
			<uni-list>
				<uni-list-item v-for="(object,i) in LianxiangResult" v-bind:key="i" style="position:relative;"  :title=object.name  @click="keywordsClick(object)" show-arrow="false"></uni-list-item>
			</uni-list>
			<uni-list v-if="haveFamily">
				<uni-list-item v-for="(object,i) in YanshengwuResult" v-bind:key="i" style="position:relative;"  :title=object.name  @click="keywordsClick(object)" show-arrow="false"></uni-list-item>
			</uni-list>
			<uni-list>
				<uni-list-item v-for="(object,i) in QuanpinResult" v-bind:key="i" style="position: relative;"  :title=object.name  @click="keywordsClick(object)" show-arrow="false"></uni-list-item>
			</uni-list>
		</view>
		<view v-if="!ShowWords&&ShowList">
		<template v-if="isBlock">
			<view class="s-block" v-if="hList.length > 0">
				<view class="header">
					历史记录
					<image src="../../static/lajitong.svg" mode="aspectFit" @click="delhistory"></image>
				</view>
				<view class="list">
					<view v-for="(item,index) in hList" :key="index" @click="InstantSearch(item)">{{item}}</view>
				</view>
			</view>
			<view class="s-block">
				<template>
				<view class="header">最热搜索
				<image src="../../static/shuaxin.svg" mode="aspectFit" @click="HotWordUpdate()"></image>
				</view>
				</template>
				<view class="list">
					<view v-for="(item,index) in wantList" :key="index" @click="InstantSearch(item)">{{item}}</view>
				</view>
			</view>
		</template>
		<template v-else>
			<view class="s-circle" v-if="hList.length > 0">
				<view class="header">
					历史记录
					<image src="../../static/lajitong.svg" mode="aspectFit" @click="delhistory"></image>
				</view>
				<view class="list">
					<view v-for="(item,index) in hList" :key="index" @click="InstantSearch(item)">{{item}}</view>
				</view>
			</view>
			<view class="s-circle">
				<template>
				<view class="header">最热搜索
				<image src="../../static/shuaxin.svg" mode="aspectFit" @click="HotWordUpdate()"></image>
				</view>
				</template>
				<view class="list">
					<view v-for="(item,index) in wantList" :key="index" @click="InstantSearch(item)">{{item}}</view>
				</view>
			</view>
		</template>
	</view>
	</view>
</template>

<script>
	import uniList from '@/components/uni-list/uni-list.vue';
	import uniListItem from '@/components/uni-list-item1/uni-list-item1.vue';
	export default{
		components: {uniList,uniListItem},
		name:"zy-search",
		props:{
			isBlock:{	//选择块级显示还是圆形显示
				type:Boolean,
				value:false
			},
			showWant:{	//是否展示推荐菜单
				type:Boolean,
				value:true
			}
		},
		data() {
			return {
				input_width:'300upx',
				searchText:'',								//搜索关键词
				hList:uni.getStorageSync('search_cache'),		//历史记录
				wantList:[],//初始化推荐列表
				collect_list:uni.getStorageSync('collect_list'),//收藏
				Languages:['普通话','粤语','河南话'],//定义语音识别种类
				SelectedLanguage:"普通话",//默认使用普通话
				SelectedLanguageCode:'zh-cn',
				ShowWords:false,
				ShowList:true,
				readyToSearch:false,//准备展示搜索结果
				haveFamily:false,//是否显示衍生物结果
				ExactResult:[],//精确结果
				LianxiangResult:[],//联想搜索结果
				QuanpinResult:[],//全拼搜索结果
				FamilyName:'',//衍生物系列类别名
				YanshengwuResult:[],//衍生物系列搜索结果
				phonenumber:"13000000000"
			}
		},
		onHide:function(){
			var Result=this.CleanUpWhenBackListener();
			uni.showTabBar({
			});
		},
		onReady:function() {
			let _this=this;
			uni.request({
				url:"https://www.shilezhihhh.cn/API/A10001",
				dataType:"text",
				responseType:"text",
				success:function(res){
					let obj=JSON.parse(res.data);
					for(var i=0;i<obj.results.length;i++){
						_this.wantList.push(obj.results[i].name);
					}
				}
			});
		},
		onBackPress:function(event){
			let _this=this;
			console.log("HERE!!!");
			_this.CleanUpWhenBackListener();
			return true;
		},
		methods: {
			cancel:function(){
				uni.navigateBack({
					url:"../home/home"
				})
			},
			searchStart() {	//触发搜索
				let _this = this;
				if (_this.searchText == '') {
					uni.showToast({
						title: '请输入关键字',
						icon: 'none',
						duration: 1000
					});
					return false;
				}else{
					uni.getStorage({
						key:'collect_list',
						success(res){
							
						}
					});
					uni.getStorage({
						key:'search_cache',
						success(res){
							let list = res.data;
							console.log(list);
							if(list.length > 5){
								for(let item of list){
									if(item == _this.searchText){
										return false;
									}
								}
								list.pop();
								list.unshift(_this.searchText);
							}else{
								for(let item of list){
									if(item == _this.searchText){
										return false;
									}
								}
								list.unshift(_this.searchText);
							}
							_this.hList = list;
							uni.setStorage({
								key: 'search_cache',
								data: _this.hList
							});
						},
						fail() {
							_this.hList = [];
							_this.hList.push(_this.searchText);
							uni.setStorage({
								key: 'search_cache',
								data: _this.hList
							});
						}
					})
				}
			},
			CleanUpWhenBackListener:function(){
				let _this=this;
				_this.searchText='';
				if(_this.ShowWords){
					_this.ShowWords=false;
					_this.ExactResult=[];
					_this.QuanpinResult=[];
					_this.YanshengwuResult=[];
					_this.LianxiangResult=[];
					_this.FamilyName='';
				}
			},
			HideEveryThing:function(){
				let _this=this;
				_this.ShowList=true;
			},
			ShowEverything:function(){
				let _this=this;
				_this.ShowWords=false;
				uni.showTabBar({
					
				});
			},
			StartToSearchWithImage:function(){
				let _this=this
				uni.chooseImage({
					count:1,
					success:function(data){
						plus.io.resolveLocalFileSystemURL(data.tempFilePaths[0], function(entry){  
						        entry.file(function(file){  
						            var reader = new plus.io.FileReader();  
						            reader.onloadend = function (e) {
										uni.request({
											url:"https://www.shilezhihhh.cn/ARTICLESYSTEM/GSPhotoUpload",
											data:{
												filename:(new Date().getTime())+".jpg",
												context:e.target.result.substring(23,e.target.result.length)
											},
											header:{
												'content-type':'application/x-www-form-urlencoded'
											},
											method:"POST",
											success:function(res1){
												console.log(res1.data)
												console.log(res1.data.url)
												var Url=res1.data.url
												uni.showLoading({
													title:"正在识别，\n请稍后...",
													mask:true
												})
												uni.request({
													url:"https://www.shilezhihhh.cn/API/A10013",
													data:{
														data:{
															url:Url
														}
													},
													method:"GET",
													success:function(res2){
														uni.hideLoading()
														//HERE!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
														console.log(res2.data)
														var obj=res2.data
														console.log(typeof(obj))
														_this.FamilyName=obj.familyname;
														if( obj.familyname==""){
															_this.haveFamily=false;
														}else{
															_this.haveFamily=true;
														}
														_this.ExactResult=[];
														_this.QuanpinResult=[];
														_this.LianxiangResult=[];
														_this.YanshengwuResult=[];
														var names=[];
														var HaveExactResults=false;
														for(var index=0,i=0;index<obj.jingque.length;index++){
															HaveExactResults=true;
															names.push(obj.jingque[index].name);
															_this.ExactResult.push(obj.jingque[i++]);
														}
														for(var index=0,i=0;index<obj.lianxiang.length;index++){
															if(HaveExactResults){
																if(obj.lianxiang[index].name==obj.jingque[0].name){
																continue;
																}
															}
															names.push(obj.lianxiang[index].name);
															_this.LianxiangResult.push(obj.lianxiang[i++]);
														}
														for(var index=0,i=0;index<obj.yanshengwu.length;index++){
															var NotHere=true;
															names.push(obj.yanshengwu[index].name);
															for(var x=0;x<names.length;x++){
																if(names[x]==obj.yanshengwu[index].name){
																	NotHere=false;
																	break;
																}
															}
															if(NotHere){
															names.push(obj.yanshengwu[index].name);
															_this.YanshengwuResult.push(obj.yanshengwu[i++]);
															}
														}
														for(var index=0,i=0;index<obj.quanpin.length;index++){
															var NotHere=true;
															for(var x=0;x<names.length;x++){
																if(names[x]==obj.quanpin[index].name){
																	NotHere=false;
																	break;
																}
															}
															if(NotHere){
															names.push(obj.quanpin[index].name);
															_this.QuanpinResult.push(obj.quanpin[i++]);
															}
														}
														_this.ShowWords=true
													},fail:function(){
														uni.hideLoading()
														uni.showToast({
															title:"服务器开小差了，\n请稍后重试...",
															icon:false,
															mask:true
														})
													}
												})
											},
											fail:function(){
												uni.hideLoading()
												uni.showToast({
													title:"图片上传失败，\n请检查网络！",
													icon:"none",
													mask:true
												})
											}
										})
						            };  
						            reader.readAsDataURL(file);  
						        },function(e){  
									uni.hideLoading()
						            mui.toast("读写出现异常: " + e.message );  
						        })  
						    })  

					},
					fail:function(){
						uni.showToast({
							title:"读取图片失败！",
							icon:"none"
						})
					}
				})
			},
			inputCallback:function(event){
				let _this=this;
				if(event.target.value==''){
					_this.ShowWords=false;
					_this.readyToSearch=false;
					return;
				}else{
					_this.ShowWords=true;
					_this.readyToSearch=true;
				}
				_this.ExactResult=[];
				_this.QuanpinResult=[];
				_this.LianxiangResult=[];
				_this.YanshengwuResult=[];
				uni.request({
					url:"https://www.shilezhihhh.cn/API/A10000",
					dataType:"text",
					responseType:"text",
					data:{
						data:"{\"input\":\""+event.target.value+"\"}"
					},
					success:function(res){
					let obj=JSON.parse(res.data);
					_this.FamilyName=obj.familyname;
					if( obj.familyname==""){
						_this.haveFamily=false;
					}else{
						_this.haveFamily=true;
					}
					var names=[];
					var HaveExactResults=false;
					for(var index=0,i=0;index<obj.jingque.length;index++){
						HaveExactResults=true;
						names.push(obj.jingque[index].name);
						_this.ExactResult.push(obj.jingque[i++]);
					}
					for(var index=0,i=0;index<obj.lianxiang.length;index++){
						if(HaveExactResults){
							if(obj.lianxiang[index].name==obj.jingque[0].name){
							continue;
							}
						}
						names.push(obj.lianxiang[index].name);
						_this.LianxiangResult.push(obj.lianxiang[i++]);
					}
					for(var index=0,i=0;index<obj.yanshengwu.length;index++){
						var NotHere=true;
						names.push(obj.yanshengwu[index].name);
						for(var x=0;x<names.length;x++){
							if(names[x]==obj.yanshengwu[index].name){
								NotHere=false;
								break;
							}
						}
						if(NotHere){
						names.push(obj.yanshengwu[index].name);
						_this.YanshengwuResult.push(obj.yanshengwu[i++]);
						}
					}
					for(var index=0,i=0;index<obj.quanpin.length;index++){
						var NotHere=true;
						for(var x=0;x<names.length;x++){
							if(names[x]==obj.quanpin[index].name){
								NotHere=false;
								break;
							}
						}
						if(NotHere){
						names.push(obj.quanpin[index].name);
						_this.QuanpinResult.push(obj.quanpin[i++]);
						}
					}
				}
				});	
					
					
			},
			InstantSearch:function(item){
				let _this=this;
				_this.searchText=item;
				var result=this.Search();
			},
			HotWordUpdate(){
				let _this=this;
				_this.wantList=[];
				uni.request({
					url:"https://www.shilezhihhh.cn/API/A10001",
					dataType:"text",
					responseType:"text",
					success:function(res){
						let obj=JSON.parse(res.data);
						for(var i=0;i<obj.results.length;i++){
							_this.wantList.push(obj.results[i].name);
						}
						uni.showToast({
						title:"热词已更新"	
						});
					},
					fail:function(){
						uni.showToast({
							title: '请检查网络连接',
							icon: 'none',
							duration: 1000
						});
					}
				});
			},
			Search(){
				let _this=this;
				if (_this.searchText == '') {
					uni.showToast({
						title: '请输入关键字',
						icon: 'none',
						duration: 1000
					});
					_this.ShowWords=false;
					return false;
				}else{
					uni.getStorage({
						key:'search_cache',
						success(res){
							let list = res.data;
							console.log(list);
							if(list.length > 5){
								for(var item of list){
									if(item == _this.searchText){
										return false;
									}
								}
								list.pop();
								list.unshift(_this.searchText);
							}else{
								for(let item of list){
									if(item == _this.searchText){
										return false;
									}
								}
								list.unshift(_this.searchText);
							}
							_this.hList = list;
							uni.setStorage({
								key: 'search_cache',
								data: _this.hList
							});
						},
						fail() {
							_this.hList = [];
							_this.hList.push(_this.searchText);
							uni.setStorage({
								key: 'search_cache',
								data: _this.hList
							});
						}
					})
				}
				var res=_this.ShowEverything();
				uni.showTabBar({
					
				});
				_this.ExactResult=[];
				_this.QuanpinResult=[];
				_this.LianxiangResult=[];
				_this.YanshengwuResult=[];
				uni.navigateTo({
					url:"../../pages/DetailedSearch/DetailedSearch?keyword="+_this.searchText
				});
			},
			keywordsClick (item) {	//进入详细信息界面
			let _this=this;
			_this.searchText=item.name;
			_this.ExactResult=[];
			_this.QuanpinResult=[];
			_this.LianxiangResult=[];
			_this.YanshengwuResult=[];
			uni.showTabBar({
				
			});
			var Result=_this.Search();
			uni.request({
				url:"https://www.shilezhihhh.cn/API/A10007",
				data:{
					data:"{\"id\":\""+item.id+"\"}"
				}
			});
			uni.getStorage({
				key:"tel",
				success(res) {
					_this.phonenumber = res.data;
					uni.request({
						url:"https://www.shilezhihhh.cn/API/A10010",
						data:{
							data:"{\"id\":\""+item.id+"\",\"phonenumber\":\""+_this.phonenumber+"\"}"
						}
					});
				}
			});
			},
			delhistory () {		//清空历史记录
			uni.showToast({
				title:"历史记录已清除"
			})
				this.hList = [];
				uni.setStorage({
					key: 'search_cache',
					data: []
				});
			},
			
			LanguagesChanged(e){//改变当前语言模式
				let _this=this;
				let MyIndex=e.target.value;
				_this.SelectedLanguage=_this.Languages[MyIndex];
				let code='';
				switch(_this.SelectedLanguage){
					case '普通话':{
						code='zh-cn';
						break;
					}
					case '粤语':{
						code='zh-cantonese';
						break;
					}
					case '河南话':{
						code='zh-henanese';
						break;
					}
					default:{
						code='zh-cn';
					}
				}
				_this.SelectedLanguageCode=code;
			},
			startRecognize: function() {	//语音输入
				let _this = this;
				var thiscode='zh-cn';
				uni.getStorage({
					key:"AudioLanguage",
					success:function(res){
						switch(res.data){
							case '普通话':{
								thiscode='zh-cn';
								break;
							}
							case '粤语':{
								thiscode='zh-cantonese';
								break;
							}
							case '河南话':{
								thiscode='zh-henanese';
								break;
							}
							case '上海话':{
								thiscode='shanghainese';
								break;
							}
							case '四川话':{
								thiscode='lmz';
								break;
							}
							default:{
								thiscode='zh-cn';
							}
						}
					},
					fail:function(){
						thiscode='zh-cn';
					}
				});
				let options = {};
				options.engine = 'iFly';
				options.punctuation = false; // 是否需要标点符号 
				options.timeout = 10 * 1000;
				options.lang=thiscode;//传入语言模式代码
				plus.speech.startRecognize(options, function(s) {
					_this.searchText = _this.searchText + s;
				});
			}
		}
	};
</script>

<style lang="less" scoped>
	.search{
		width: 592upx;
		height: 72upx;
		margin: 70upx 32upx 0upx 32upx;
		position: relative;
		//background-color: #999999;
		input{
			background-color: #F3F4F7;
			padding-left: 90upx;
			height: 68upx;
			font-size: 28upx;
			border-radius: 36upx;
			caret-color: #2EC4B6;
			border: 1upx solid #2EC4B6;
		}
		.voice-icon{
			width: 40upx;
			height: 40upx;
			position: absolute;
			top: 16upx;
			left:30upx;
			z-index: 10;
		}
		.image-icon{
			width: 40upx;
			height: 40upx;
			position: absolute;
			top: 16upx;
			left:475upx;
			z-index: 10;
		}
		.search-icon{
			width: 40upx;
			height: 40upx;
			position: absolute;
			top:16upx;
			left:526upx;
			z-index: 10;
		}
		.delete-icon{
			width: 40upx;
			height: 40upx;
			position: absolute;
			left: 522upx;
			top: 16upx;
			z-index: 10;
		}
	}
	.cancel{
		font-size: 32upx;
		color: #333333;
		position: absolute;
		top: 84upx;
		right: 32upx;
	}
	.s-block{
		//margin-top: 50upx;
		.header{
			font-size: 32upx;
			padding: 40upx 0upx 0upx 32upx;
			color: #333333;
			position: relative;
			image{
				width: 36upx;
				height: 36upx;
				padding: 17upx;
				position: absolute;
				right: 40upx;
				top: 24upx;
			}
		}
		.list{
			display: inline-flex;
			flex-wrap: wrap;
			margin:40upx 0upx 40upx 32upx;
			view{
				font-size: 28upx;
				box-sizing: border-box;
				text-align: center;
				padding: 18upx 30upx;
				margin: 10upx 10upx;
				overflow: hidden;
				white-space: nowrap;
				text-overflow: ellipsis;
				background-color: #F3F4F8;
				color: #333333;
				border-radius: 60upx;
			}
		}
	}
	.s-circle{
		margin-top: 30upx;
		.header{
			font-size: 32upx;
			padding: 30upx;
			border-bottom: 2upx solid #F9F9F9;
			position: relative;
			image{
				width: 36upx;
				height: 36upx;
				padding: 10upx;
				position: absolute;
				right: 40upx;
				top: 24upx;
			}
		}
		.list{
			display: flex;
			flex-wrap: wrap;
			padding: 0 30upx 20upx;
			view{
				padding: 8upx 30upx;
				margin: 20upx 30upx 0 0;
				font-size: 28upx;
				color: #8A8A8A;
				background-color: #F7F7F7;
				box-sizing: border-box;
				text-align: center;
				border-radius: 20upx;
			}
		}
	}
	.wanted-block{
		margin-top: 30upx;
		.header{
			font-size: 32upx;
			padding: 30upx;
		}
		.list{
			display: flex;
			flex-wrap: wrap;
			view{
				width: 50%;
				color: #8A8A8A;
				font-size: 28upx;
				box-sizing: border-box;
				text-align: center;
				padding: 20upx 0;
				//border-top: 2upx solid #FFF;
				//border-left: 2upx solid #FFF;
				background-color: #F7F7F7;
				overflow: hidden;
				white-space: nowrap;
				text-overflow: ellipsis;
			}
		}
	}
	.wanted-circle{
		margin-top: 30upx;
		.header{
			font-size: 32upx;
			padding: 30upx;
		}
		.list{
			display: flex;
			flex-wrap: wrap;
			padding: 0 30upx 20upx;
			view{
				padding: 8upx 30upx;
				margin: 20upx 30upx 0 0;
				font-size: 28upx;
				color: #8A8A8A;
				background-color: #F7F7F7;
				box-sizing: border-box;
				text-align: center;
				border-radius: 20upx;
			}
		}
	}
</style>
