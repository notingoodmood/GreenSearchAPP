<template>
	<view>
			<view style="z-index: 99999; position: relative;">
			<uni-nav-bar left-text="重定位" right-text="设置" :title="addressName" fixed="true" border="false" shadow="true" @click-right="toSheZhi" @click-left="getRegeo"></uni-nav-bar>
			</view>
			<view v-if="cityType == 0">
			<sun-tab :value.sync="swiperIndex" :tabList="leaderArray"></sun-tab>
			</view>
			
			<view v-if="cityType == 7">
			<sun-tab :value.sync="swiperIndex" :tabList="leaderArray1"></sun-tab>
			</view>
			
			
			<view v-if="cityType == 14">
			<sun-tab :value.sync="swiperIndex" :tabList="leaderArray2"></sun-tab>
			</view>
			
			
			<view v-if="cityType == 21">
			<sun-tab :value.sync="swiperIndex" :tabList="leaderArray3"></sun-tab>
			</view>
			
			
			<view v-if="cityType == 0">
			<swiper :current="swiperIndex" @change="swiperChange" style="height: 1400upx;">				
				<swiper-item v-for="item in tabSwiperList" :key="item.leader">
					<image :src="item.leaderImage" style="height: 250upx; margin-top: 3%;width: 100%;"></image>
					<view v-for="it in item.sum" class="outerBolck"> <!-- 遍历列表中的列表 -->
						<view class="everyBlock">
							<p class="everyHead">{{it.head}}</p>
							<p class="everyInfo">{{it.info}}</p>
							<image :src="it.url" class="everyImage">
						</view>	
					</view>
				</swiper-item>
			</swiper>
			</view>
			
			<view v-if="cityType == 7">
			<swiper :current="swiperIndex" @change="swiperChange" style="height: 1400upx;">				
				<swiper-item v-for="item in tabSwiperList1" :key="item.leader">
					<image :src="item.leaderImage" style="height: 250upx; margin-top: 3%;width: 100%;"></image>
					<view v-for="it in item.sum" class="outerBolck"> <!-- 遍历列表中的列表 -->
						<view class="everyBlock">
							<p class="everyHead">{{it.head}}</p>
							<p class="everyInfo">{{it.info}}</p>
							<image :src="it.url" class="everyImage">
						</view>	
					</view>
				</swiper-item>
			</swiper>
			</view>
			
			<view v-if="cityType == 14">
			<swiper :current="swiperIndex" @change="swiperChange" style="height: 1400upx;">				
				<swiper-item v-for="item in tabSwiperList2" :key="item.leader">
					<image :src="item.leaderImage" style="height: 250upx; margin-top: 3%;width: 100%;"></image>
					<view v-for="it in item.sum" class="outerBolck"> <!-- 遍历列表中的列表 -->
						<view class="everyBlock">
							<p class="everyHead">{{it.head}}</p>
							<p class="everyInfo">{{it.info}}</p>
							<image :src="it.url" class="everyImage">
						</view>	
					</view>
				</swiper-item>
			</swiper>
			</view>
			
			<view v-if="cityType == 21">
			<swiper :current="swiperIndex" @change="swiperChange" style="height: 1400upx;">				
				<swiper-item v-for="item in tabSwiperList3" :key="item.leader">
					<image :src="item.leaderImage" style="height: 250upx; margin-top: 3%;width: 100%;"></image>
					<view v-for="it in item.sum" class="outerBolck"> <!-- 遍历列表中的列表 -->
						<view class="everyBlock">
							<p class="everyHead">{{it.head}}</p>
							<p class="everyInfo">{{it.info}}</p>
							<image :src="it.url" class="everyImage">
						</view>	
					</view>
				</swiper-item>
			</swiper>
			</view>
			
			
			
		</view>
		
		
		
	</view>
</template>

<script>
	import sunTab from '@/components/sun-tab/sun-tab.vue';
	import amap from '../../common/amap-wx.js';
	import uniNavBar from "../../components/zolysoft-nav-bar/zolysoft-nav-bar.vue";
	import helper from "../../common/helper.js"
	//import uniNavBar from "../../components/uni-nav-bar/uni-nav-bar.vue";
	export default {
		// components: {
		// 	sunTab
		// },
		components:{uniNavBar,sunTab},
		
		
		
		
		data() {
			return {		
				index: 0,
				swiperIndex:0,
				leaderArray: ['可回收物','厨余垃圾','有害垃圾','其他垃圾'],
				leaderArray1:['可回收物','易腐垃圾','有害垃圾','其他垃圾'],
				leaderArray2:['可回收物','餐厨垃圾','有害垃圾','其他垃圾'],
				leaderArray3:['可回收物','湿垃圾','有害垃圾','干垃圾'],
				tabSwiperList: [
					{
						leader:'可回收物',
						leaderImage:'../../static/lpf_home_img/kehuishou.png',
						sum:[
							{head:'纸类',info:'未严重玷污的文字用纸、包装用纸和其他纸制品等。如报纸、各种包装纸、办公用纸、广告纸片、纸盒、复印纸等。',url:'../../static/tissue.jpg'},
							{head:'塑料',info:'废容器塑料、包装塑料等塑料制品。比如各种塑料袋、塑料瓶、泡沫塑料、一次性塑料餐盒餐具、硬塑料等。',url:'../../static/plastic.jpg'},
							{head:'金属',info:'易拉罐、金属罐头盒、装饰物、铝箔、铁片、铁钉、铁管、废铁丝、旧钢丝球、铜导线等,按照回收材料分为铁类，非铁类。',url:'../../static/metal.jpg'},
							{head:'玻璃',info:'玻璃饮料瓶、玻璃酒瓶、坏玻璃杯、碎玻璃窗、废玻璃板、镜片、镜子等，根据回收工艺，玻璃分为无色玻璃，绿色玻璃，棕色玻璃',url:'../../static/glass.jpg'}
							
						]
						
					},
					{
						leader:'厨余垃圾',
						leaderImage:'../../static/lpf_home_img/chuyu.png',
						sum:[
							{head:'香蕉皮',info:'餐厨垃圾,香蕉的表皮。富含蛋白质、脂肪、糖以及K、Ca、Mg、S、Fe、Zn等十几种元素，可作为潜在的资源与原材料。',url:'../../static/banana1.jpg'},
							{head:'面包屑',info:'面包食用后的残渣，可食部分为100%，含有丰富的膳食纤维、不饱和脂肪酸和矿物质，有助新陈代谢，有益身体健康。',url:'../../static/bread.jpg'},
							{head:'泔水',info:'城市泔水普遍采用不规范的铁桶或塑料桶等容器收集， 人力三轮车等工具运输。最好不乱扔泔水，对环境造成极大的危害。',url:'../../static/ganshui.jpg'},
							{head:'蛋壳',info:'雉科动物鸡所产卵之外壳。能制酸、止痛，研末外用可用于外伤止血、固涩收敛，服可用于胃溃疡反酸、胃炎疼痛。',url:'../../static/egg.jpg'}
						]
						
					},
					{
						leader:'有害垃圾',
						leaderImage:'../../static/lpf_home_img/youhai.png',
						sum:[
							{head:'废旧电池',info:'电池主要含镉等重金属元素，此外还含有有毒微量的汞。废旧电池中可回收杂锌锭、冶金二氧化锰、电解锌等一次性资源',url:'../../static/battery.jpg'},
							{head:'废荧光灯管',info:'即低压汞灯，是利用低气压的汞蒸气在通电后释放紫外线，从而使荧光粉发出可见光的原理发光，主要原料为三基色荧光粉',url:'../../static/light.jpg'},
							{head:'使用过的医药用品',info:'病人使用过的医疗器械,如针筒、卫生棉、注射器等具有传染危害,应有医疗机构统一灭菌废弃处理',url:'../../static/injury.jpg'},
							{head:'废油漆',info:'废油漆是使用销售过程中产生的废弃的、伪劣的、失效的、不合格的油漆，它及盛装或沾有它的容器也是危险废弃物。',url:'../../static/wasted.jpg'}
						]
						
					},
					{
						leader:'其他垃圾',
						leaderImage:'../../static/lpf_home_img/qita.png',
						sum:[
							{head:'烟头',info:'香烟抽完后剩下的烟蒂，虽危害不大但随处可见的城市垃圾，为市容市貌及环卫工人的工作带来极大困扰。',url:'../../static/cigraitty.jpg'},
							{head:'宠物粪便',info:'宠物的排泄物，宠物主人在室外时应在宠物身边，及时清理宠物的排泄物，保持良好环境。',url:'../../static/pet.jpg'},
							{head:'一次性餐具',info:'一次性餐具按原材料来源、生产工艺、降解方式、回收水平分为以下三大类： 1、生物降解类 2、光/生物降解性材料类 3、易于回收利用材料类',url:'../../static/dinner.jpg'},
							{head:'灰土',info:'灰土是中国北方传统的建筑材料之一，用黏土、石灰加水拌和夯实而成。土壤颗粒细、活性大，因此强度比砂性土壤高',url:'../../static/soild.jpg'}
						]
						
					}

				],
				tabSwiperList1: [
					{
						leader:'可回收物',
						leaderImage:'../../static/lpf_home_img/kehuishou.png',
						sum:[
							{head:'纸类',info:'未严重玷污的文字用纸、包装用纸和其他纸制品等。如报纸、各种包装纸、办公用纸、广告纸片、纸盒、复印纸等。',url:'../../static/tissue.jpg'},
							{head:'塑料',info:'废容器塑料、包装塑料等塑料制品。比如各种塑料袋、塑料瓶、泡沫塑料、一次性塑料餐盒餐具、硬塑料等。',url:'../../static/plastic.jpg'},
							{head:'金属',info:'易拉罐、金属罐头盒、装饰物、铝箔、铁片、铁钉、铁管、废铁丝、旧钢丝球、铜导线等,按照回收材料分为铁类，非铁类。',url:'../../static/metal.jpg'},
							{head:'玻璃',info:'玻璃饮料瓶、玻璃酒瓶、坏玻璃杯、碎玻璃窗、废玻璃板、镜片、镜子等，根据回收工艺，玻璃分为无色玻璃，绿色玻璃，棕色玻璃',url:'../../static/glass.jpg'}
							
						]
						
					},
					{
						leader:'易腐垃圾',
						leaderImage:'../../static/lpf_home_img/yifu.png',
						sum:[
							{head:'香蕉皮',info:'餐厨垃圾,香蕉的表皮。富含蛋白质、脂肪、糖以及K、Ca、Mg、S、Fe、Zn等十几种元素，可作为潜在的资源与原材料。',url:'../../static/banana1.jpg'},
							{head:'面包屑',info:'面包食用后的残渣，可食部分为100%，含有丰富的膳食纤维、不饱和脂肪酸和矿物质，有助新陈代谢，有益身体健康。',url:'../../static/bread.jpg'},
							{head:'泔水',info:'城市泔水普遍采用不规范的铁桶或塑料桶等容器收集， 人力三轮车等工具运输。最好不乱扔泔水，对环境造成极大的危害。',url:'../../static/ganshui.jpg'},
							{head:'蛋壳',info:'雉科动物鸡所产卵之外壳。能制酸、止痛，研末外用可用于外伤止血、固涩收敛，服可用于胃溃疡反酸、胃炎疼痛。',url:'../../static/egg.jpg'}
						]
						
					},
					{
						leader:'有害垃圾',
						leaderImage:'../../static/lpf_home_img/youhai.png',
						sum:[
							{head:'废旧电池',info:'电池主要含镉等重金属元素，此外还含有有毒微量的汞。废旧电池中可回收杂锌锭、冶金二氧化锰、电解锌等一次性资源',url:'../../static/battery.jpg'},
							{head:'废荧光灯管',info:'即低压汞灯，是利用低气压的汞蒸气在通电后释放紫外线，从而使荧光粉发出可见光的原理发光，主要原料为三基色荧光粉',url:'../../static/light.jpg'},
							{head:'使用过的医药用品',info:'病人使用过的医疗器械,如针筒、卫生棉、注射器等具有传染危害,应有医疗机构统一灭菌废弃处理',url:'../../static/injury.jpg'},
							{head:'废油漆',info:'废油漆是使用销售过程中产生的废弃的、伪劣的、失效的、不合格的油漆，它及盛装或沾有它的容器也是危险废弃物。',url:'../../static/wasted.jpg'}
						]
						
					},
					{
						leader:'其他垃圾',
						leaderImage:'../../static/lpf_home_img/qita.png',
						sum:[
							{head:'烟头',info:'香烟抽完后剩下的烟蒂，虽危害不大但随处可见的城市垃圾，为市容市貌及环卫工人的工作带来极大困扰。',url:'../../static/cigraitty.jpg'},
							{head:'宠物粪便',info:'宠物的排泄物，宠物主人在室外时应在宠物身边，及时清理宠物的排泄物，保持良好环境。',url:'../../static/pet.jpg'},
							{head:'一次性餐具',info:'一次性餐具按原材料来源、生产工艺、降解方式、回收水平分为以下三大类： 1、生物降解类 2、光/生物降解性材料类 3、易于回收利用材料类',url:'../../static/dinner.jpg'},
							{head:'灰土',info:'灰土是中国北方传统的建筑材料之一，用黏土、石灰加水拌和夯实而成。土壤颗粒细、活性大，因此强度比砂性土壤高',url:'../../static/soild.jpg'}
						]
						
					}
				
				],
				tabSwiperList2: [
					{
						leader:'可回收物',
						leaderImage:'../../static/lpf_home_img/kehuishou.png',
						sum:[
							{head:'纸类',info:'未严重玷污的文字用纸、包装用纸和其他纸制品等。如报纸、各种包装纸、办公用纸、广告纸片、纸盒、复印纸等。',url:'../../static/tissue.jpg'},
							{head:'塑料',info:'废容器塑料、包装塑料等塑料制品。比如各种塑料袋、塑料瓶、泡沫塑料、一次性塑料餐盒餐具、硬塑料等。',url:'../../static/plastic.jpg'},
							{head:'金属',info:'易拉罐、金属罐头盒、装饰物、铝箔、铁片、铁钉、铁管、废铁丝、旧钢丝球、铜导线等,按照回收材料分为铁类，非铁类。',url:'../../static/metal.jpg'},
							{head:'玻璃',info:'玻璃饮料瓶、玻璃酒瓶、坏玻璃杯、碎玻璃窗、废玻璃板、镜片、镜子等，根据回收工艺，玻璃分为无色玻璃，绿色玻璃，棕色玻璃',url:'../../static/glass.jpg'}
							
						]
						
					},
					{
						leader:'餐厨垃圾',
						leaderImage:'../../static/lpf_home_img/canchu.png',
						sum:[
							{head:'香蕉皮',info:'餐厨垃圾,香蕉的表皮。富含蛋白质、脂肪、糖以及K、Ca、Mg、S、Fe、Zn等十几种元素，可作为潜在的资源与原材料。',url:'../../static/banana1.jpg'},
							{head:'面包屑',info:'面包食用后的残渣，可食部分为100%，含有丰富的膳食纤维、不饱和脂肪酸和矿物质，有助新陈代谢，有益身体健康。',url:'../../static/bread.jpg'},
							{head:'泔水',info:'城市泔水普遍采用不规范的铁桶或塑料桶等容器收集， 人力三轮车等工具运输。最好不乱扔泔水，对环境造成极大的危害。',url:'../../static/ganshui.jpg'},
							{head:'蛋壳',info:'雉科动物鸡所产卵之外壳。能制酸、止痛，研末外用可用于外伤止血、固涩收敛，服可用于胃溃疡反酸、胃炎疼痛。',url:'../../static/egg.jpg'}
						]
						
					},
					{
						leader:'有害垃圾',
						leaderImage:'../../static/lpf_home_img/youhai.png',
						sum:[
							{head:'废旧电池',info:'电池主要含镉等重金属元素，此外还含有有毒微量的汞。废旧电池中可回收杂锌锭、冶金二氧化锰、电解锌等一次性资源',url:'../../static/battery.jpg'},
							{head:'废荧光灯管',info:'即低压汞灯，是利用低气压的汞蒸气在通电后释放紫外线，从而使荧光粉发出可见光的原理发光，主要原料为三基色荧光粉',url:'../../static/light.jpg'},
							{head:'使用过的医药用品',info:'病人使用过的医疗器械,如针筒、卫生棉、注射器等具有传染危害,应有医疗机构统一灭菌废弃处理',url:'../../static/injury.jpg'},
							{head:'废油漆',info:'废油漆是使用销售过程中产生的废弃的、伪劣的、失效的、不合格的油漆，它及盛装或沾有它的容器也是危险废弃物。',url:'../../static/wasted.jpg'}
						]
						
					},
					{
						leader:'其他垃圾',
						leaderImage:'../../static/lpf_home_img/qita.png',
						sum:[
							{head:'烟头',info:'香烟抽完后剩下的烟蒂，虽危害不大但随处可见的城市垃圾，为市容市貌及环卫工人的工作带来极大困扰。',url:'../../static/cigraitty.jpg'},
							{head:'宠物粪便',info:'宠物的排泄物，宠物主人在室外时应在宠物身边，及时清理宠物的排泄物，保持良好环境。',url:'../../static/pet.jpg'},
							{head:'一次性餐具',info:'一次性餐具按原材料来源、生产工艺、降解方式、回收水平分为以下三大类： 1、生物降解类 2、光/生物降解性材料类 3、易于回收利用材料类',url:'../../static/dinner.jpg'},
							{head:'灰土',info:'灰土是中国北方传统的建筑材料之一，用黏土、石灰加水拌和夯实而成。土壤颗粒细、活性大，因此强度比砂性土壤高',url:'../../static/soild.jpg'}
						]
						
					}
				
				],
				tabSwiperList3: [
					{
						leader:'可回收物',
						leaderImage:'../../static/lpf_home_img/kehuishou.png',
						sum:[
							{head:'纸类',info:'未严重玷污的文字用纸、包装用纸和其他纸制品等。如报纸、各种包装纸、办公用纸、广告纸片、纸盒、复印纸等。',url:'../../static/tissue.jpg'},
							{head:'塑料',info:'废容器塑料、包装塑料等塑料制品。比如各种塑料袋、塑料瓶、泡沫塑料、一次性塑料餐盒餐具、硬塑料等。',url:'../../static/plastic.jpg'},
							{head:'金属',info:'易拉罐、金属罐头盒、装饰物、铝箔、铁片、铁钉、铁管、废铁丝、旧钢丝球、铜导线等,按照回收材料分为铁类，非铁类。',url:'../../static/metal.jpg'},
							{head:'玻璃',info:'玻璃饮料瓶、玻璃酒瓶、坏玻璃杯、碎玻璃窗、废玻璃板、镜片、镜子等，根据回收工艺，玻璃分为无色玻璃，绿色玻璃，棕色玻璃',url:'../../static/glass.jpg'}
							
						]
						
					},
					{
						leader:'湿垃圾',
						leaderImage:'../../static/lpf_home_img/shi.png',
						sum:[
							{head:'香蕉皮',info:'餐厨垃圾,香蕉的表皮。富含蛋白质、脂肪、糖以及K、Ca、Mg、S、Fe、Zn等十几种元素，可作为潜在的资源与原材料。',url:'../../static/banana1.jpg'},
							{head:'面包屑',info:'面包食用后的残渣，可食部分为100%，含有丰富的膳食纤维、不饱和脂肪酸和矿物质，有助新陈代谢，有益身体健康。',url:'../../static/bread.jpg'},
							{head:'泔水',info:'城市泔水普遍采用不规范的铁桶或塑料桶等容器收集， 人力三轮车等工具运输。最好不乱扔泔水，对环境造成极大的危害。',url:'../../static/ganshui.jpg'},
							{head:'蛋壳',info:'雉科动物鸡所产卵之外壳。能制酸、止痛，研末外用可用于外伤止血、固涩收敛，服可用于胃溃疡反酸、胃炎疼痛。',url:'../../static/egg.jpg'}
						]
						
					},
					{
						leader:'有害垃圾',
						leaderImage:'../../static/lpf_home_img/youhai.png',
						sum:[
							{head:'废旧电池',info:'电池主要含镉等重金属元素，此外还含有有毒微量的汞。废旧电池中可回收杂锌锭、冶金二氧化锰、电解锌等一次性资源',url:'../../static/battery.jpg'},
							{head:'废荧光灯管',info:'即低压汞灯，是利用低气压的汞蒸气在通电后释放紫外线，从而使荧光粉发出可见光的原理发光，主要原料为三基色荧光粉',url:'../../static/light.jpg'},
							{head:'使用过的医药用品',info:'病人使用过的医疗器械,如针筒、卫生棉、注射器等具有传染危害,应有医疗机构统一灭菌废弃处理',url:'../../static/injury.jpg'},
							{head:'废油漆',info:'废油漆是使用销售过程中产生的废弃的、伪劣的、失效的、不合格的油漆，它及盛装或沾有它的容器也是危险废弃物。',url:'../../static/wasted.jpg'}
						]
						
					},
					{
						leader:'干垃圾',
						leaderImage:'../../static/lpf_home_img/gan.png',
						sum:[
							{head:'烟头',info:'香烟抽完后剩下的烟蒂，虽危害不大但随处可见的城市垃圾，为市容市貌及环卫工人的工作带来极大困扰。',url:'../../static/cigraitty.jpg'},
							{head:'宠物粪便',info:'宠物的排泄物，宠物主人在室外时应在宠物身边，及时清理宠物的排泄物，保持良好环境。',url:'../../static/pet.jpg'},
							{head:'一次性餐具',info:'一次性餐具按原材料来源、生产工艺、降解方式、回收水平分为以下三大类： 1、生物降解类 2、光/生物降解性材料类 3、易于回收利用材料类',url:'../../static/dinner.jpg'},
							{head:'灰土',info:'灰土是中国北方传统的建筑材料之一，用黏土、石灰加水拌和夯实而成。土壤颗粒细、活性大，因此强度比砂性土壤高',url:'../../static/soild.jpg'}
						]
						
					}
				
				],
				
				
				//定位相关
				hasLocation: false,
				location: {},
				amapPlugin: null,
				key: 'f08cf557fe314b3012dd5ebd5cb47ea9',
				addressName: '成都',
				weather: {
					hasData: false,
					data: []
				},
				cityType:-1
				
			}
		},
		onLoad() {
			var that = this;
			
			
			
			
			
			
			
			
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
				{	
					that.cityType = 0;
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
				{	
					that.cityType=7;
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
				{	
					that.cityType=14;
					break;
				}
				case "邯郸":
				case "上海":
				{	
					
					that.cityType=21;
					break;
				}
				default:
				{	
					
					that.cityType=7;
					break;
				}
			}
			
			
		},
		onShow() {
			var that = this;
			uni.getStorage({
				key:'City',
				success:function(res){
					that.addressName = res.data
				}
			});
			var localcity = that.addressName;
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
				{	
					that.cityType = 0;
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
				{	
					that.cityType=7;
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
				{	
					that.cityType=14;
					break;
				}
				case "邯郸":
				case "上海":
				{	
					
					that.cityType=21;
					break;
				}
				default:
				{	
					
					that.cityType=7;
					break;
				}
			}
			
			
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
				
			},
			getRegeo() {
				uni.showLoading({
					title: '获取信息中'
				});
				var that = this
				uni.getLocation({
					'type': 'wgs84',
					'geocode':'true',
				    success: function (res) {
				       
				        console.log('详细地址：' + res.address.city);
						var tmp = res.address.city;
						console.log(tmp);
						console.log(tmp.slice(0,tmp.length - 1));
						
						console.log("adad");
						console.log(tmp)
						uni.setStorage({
							key:'City',
							data:tmp.slice(0,tmp.length - 1)
						})
						
						uni.getStorage({
							key:'City',
							success:function(res){
								that.addressName = res.data
							}
						})
						
						
						
						
						// that.addressName = tmp.slice(0,tmp.length - 1);
				        console.log('纬度：' + res.latitude);
				        console.log('经度：' + res.longitude);
						uni.hideLoading();
						
						var localcity="成都";
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
							{	
								that.cityType = 0;
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
							{	
								that.cityType=7;
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
							{	
								that.cityType=14;
								break;
							}
							case "邯郸":
							case "上海":
							{	
								
								that.cityType=21;
								break;
							}
							default:
							{	
								
								that.cityType=7;
								break;
							}
						}
				    }
				});
				
			},
			getWeather() {
				uni.showLoading({
					title: '获取信息中',
					
				});
				this.amapPlugin.getWeather({
					success: (data) => {
						console.log(data);
						for (const key in data) {
							console.log(key);
							if (key !== 'liveData') {
								this.weather.data.push({
									name: 'div',
									children: [{
										type: 'text',
										text: data[key].text + '：' + data[key].data
									}]
								});
							}
						}
						uni.hideLoading();
						this.weather.hasData = true;
					}
				});
			},
			toSheZhi(){
				console.log("hkladhila");
				uni.switchTab({
					url:'../Settings/Settings'
				})
			}
			
			
		}
	}
</script>
<style>
	.chooseTrash{
		margin-left: 50upx;
	}
	//所有框的整体
	.outerBolck{
		margin-top: 30upx;
		
	}
	.everyBlock{
		padding-left: 2%;
		position: relative;
		border-style: solid;
		border-width: 2upx;
		border-color: #F1F1F1;
			
	}
	.everyHead{
		margin-left: 3%;
		font-size: 40upx;
		margin-bottom: 2%;
	}
	.everyInfo{
		text-indent: 40upx;
		margin-right: 180upx;
		margin-left: 10upx;
		font-size: 30upx;
	}
	.swiper_every{
		width: 100%;
	}
	.everyImage{
		margin-left: 78%;
		margin-top: 6%;
		top: 0;
		position: absolute;
		height: 120upx;
		width: 120upx;
	}

</style>
