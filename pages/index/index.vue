<template>
	<view>		
		<!-- 分类选项 -->
		<view class="uni-tab-bar">
			<scroll-view scroll-x="true" class="uni-swiper-tab" id="scroll-view-classfication">
				<block v-for="(tab,index) in tabBars" :key="tab.id">
					<view class="swiper-tab-list class-item" :class="{'active':tabIndex == index}" @tap="tabtap(index)">
					{{tab.name}} 
					</view>
				</block>
			</scroll-view>
			<!-- 当前分类不是推荐选项的时候，显示下面分类条件 -->
			<view class="order-condition-wrap" v-if="tabIndex != 0">
				<view class="order-condition active">按时间</view>
				<view class="order-condition">按热度</view>
			</view>	
		</view>
		<view class="uni-tab-bar">
			<swiper class="swiper-box" :style="{height:swiperHeight+'px'}" :current="tabIndex" @change="tabChange">
				<swiper-item v-for="(items,index) in newsList" :key="index">
					
					<!-- 推荐布局 -->
					<scroll-view v-if="index==0" scroll-y="true" class="list">
						<!-- 轮播图 -->
						<view class="uni-padding-wrap" style="padding: 0;width: 100%;">
							<view class="page-section swiper">
								<view class="page-section-spacing">
									<swiper class="swiper" :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration">
										<swiper-item v-for="(swiperItem,index) in items.swiperList" :key="index">
											<text>{{swiperItem.title}}</text>
											<image :src="swiperItem.pic" mode=""></image>
										</swiper-item>
									</swiper>
								</view>
							</view>
						</view>	
						<!-- 精彩推荐 -->
						<index-recommend-list sectionName="精彩推荐" itemWidth=140 temHeight=100 :list="recommondList"></index-recommend-list>
						<!-- 最新上新 -->
						<index-recommend-list sectionName="最新上新" itemWidth=95 itemHeight=150 :list="latestList"></index-recommend-list>
					</scroll-view>
					
					<!-- 推荐之外的分类布局 --> 
					<scroll-view v-else scroll-y="true" class="list" @scrolltolower="loadmore(index)">
						<!-- 图文列表 -->
						<block v-for="(item,index1) in items.list" :key="index1" >
							<index-class-list :item="item" :index="index1"></index-class-list>
						</block>
						<!-- 加载更多 -->
						<view class="load-more">{{items.loadtext}}</view>
					</scroll-view>
				</swiper-item>
			</swiper>
		</view>
	</view>
</template>

<script>
	import indexClassList from "@/components/index/index-class-list/index-class-list.vue"
	import indexRecommendList from "@/components/index/index-recommend-list/index-recommend-list.vue"
	export default {
		components:{
			indexClassList,
			indexRecommendList
		},
		methods: {
			//上拉加载
			loadmore(index){
				if(this.newsList[index].loadtext!="上拉加载更多"){
					return;
				}
				this.newsList[index].loadtext="加载中...";
				setTimeout(()=>{
					//获取完成
					let obj={
						title:"小白兔与大灰狼",
						pic:"../../../static/balloon.jpg",
						describe:"这是小白兔与大灰狼的故事",
						releaseTime:"2020.2.18" 
					}
					this.newsList[index].list.push(obj);
					this.newsList[index].loadtext="上拉加载更多";
				},1000);
				// this.newsList[index].loadtext="没有更多了";
			},
			//点击事件
			tabtap(index){
				this.tabIndex = index;
			},
			//滑动事件
			tabChange(e){
				this.tabIndex = e.detail.current;
				console.log(JSON.stringify(e.detail));
			}
		},
		//原生导航栏按钮监听
		onNavigationBarButtonTap(e){
			console.log(JSON.stringify(e));
			//导航栏左边按钮
			if(e.index == 0){
				uni.navigateTo({
					url:"../login/login"
				});
			} else {
				console.log("搜索");
			}
		},
		onLoad() {
			this.isLogin = false;
			//判断是否登陆，根据情况改变leftTexr内容
			if(this.isLogin){
				// #ifdef APP-PLUS
				var webView = this.$mp.page.$getAppWebview();  
				 
				// 修改buttons  
				// index: 按钮索引, style {WebviewTitleNViewButtonStyles }  
				webView.setTitleNViewButtonStyle(0, {  
				    text: ' ',  
					width:"0px"
				});   
				console.log(111);
				// #endif  
				// this.leftText = "登陆后享更多特权",
				
			};
			uni.getSystemInfo({
				success:(res)=> {
					let _self = this;
					//获取分类导航栏的高度
					let info = uni.createSelectorQuery().select(".uni-tab-bar");
			　　　  		info.boundingClientRect(function(data) { //data - 各种参数
						//data.hieght为整个屏幕的高度
						_self.swiperHeight = res.windowHeight-data.height;
			　　    }).exec();
				}
			})
		},
		data() {
			return {  
				isLogin:Boolean,
				tabIndex: 0,
				swiperHeight: 500,
				recommondList:[
					{
						title:"小红帽",
						pic:"../../../static/balloon.jpg",
						describe:"这是小白兔与大灰狼的故事"
					},
					{
						title:"大灰狼",
						pic:"../../../static/balloon.jpg",
						describe:"这是小白兔与大灰狼的故事"
					}
				],
				latestList:[
					{
						title:"小红帽",
						pic:"../../../static/balloon.jpg",
						describe:"这是小白兔与大灰狼的故事"
					},
					{
						title:"小红帽",
						pic:"../../../static/balloon.jpg",
						describe:"这是小白兔与大灰狼的故事"
					},
					{
						title:"小白兔",
						pic:"../../../static/balloon.jpg",
						describe:"这是小白兔与大灰狼的故事"
					}
				],
				tabBars: [
					{name:"推荐",id:"tuijian"},
					{name:"全部分类",id:"quanbu"},
					{name:"童话",id:"tonghua"},
					{name:"搞笑",id:"gaoxiao"},
					{name:"寓言",id:"yuyan"}
				],
				newsList:[
					{	
						swiperList:[
							{
								title:"小白兔与大灰狼",
								pic:"../../../static/balloon.jpg",
								describe:"这是小白兔与大灰狼的故事"
							},
							{
								title:"小白兔与大灰狼",
								pic:"../../../static/balloon.jpg",
								describe:"这是小白兔与大灰狼的故事",
							},
							{
								title:"小白兔与大灰狼",
								pic:"../../../static/balloon.jpg",
								describe:"这是小白兔与大灰狼的故事",
							}
						],
						recommend:[
							{
								title:"小白兔与大灰狼",
								pic:"../../../static/balloon.jpg",
								describe:"这是小白兔与大灰狼的故事",
							},
							{
								title:"小白兔与大灰狼",
								pic:"../../../static/balloon.jpg",
								describe:"这是小白兔与大灰狼的故事",
							}
						],
						latestList:[],
						loadtext:"上拉加载更多",
						list:[
							{
								title:"小白兔与大灰狼",
								pic:"../../../static/balloon.jpg",
								describe:"这是小白兔与大灰狼的故事",
								releaseTime:"2020.2.18" 
							},
							{
								title:"小白兔与大灰狼",
								pic:"../../../static/balloon.jpg",
								describe:"这是小白兔与大灰狼的故事",
								releaseTime:"2020.2.18" 
							},
							{
								title:"12345",
								pic:"../../../static/balloon.jpg",
								describe:"这是小白兔与大灰狼的故事",
								releaseTime:"2020.2.18" 
							}
						]
					},
					{
						loadtext:"上拉加载更多",
						list:[
							{
								title:"小白兔与大灰狼",
								pic:"../../../static/balloon.jpg",
								describe:"这是小白兔与大灰狼的故事",
								releaseTime:"2020.2.18" 
							},
							{
								title:"小白兔与大灰狼",
								pic:"../../../static/balloon.jpg",
								describe:"这是小白兔与大灰狼的故事",
								releaseTime:"2020.2.18" 
							}
						]
					},
					{
						loadtext:"上拉加载更多",
						list:[]
					},
					{
						loadtext:"上拉加载更多",
						list:[]
					},
					{
						loadtext:"上拉加载更多",
						list:[]
					}
				]
			}
		}
	}

</script>

<style> 
page{
	background-color: #EEEEEE;
}
.order-condition-wrap {
	font-size: 16px;
	color: #737373;
}
.order-condition-wrap .order-condition {
	display: inline-block;
	margin-left: 10px;
	width: 66px;
	padding: 2px 0;
	text-align: center;
}
.order-condition-wrap .active {
	color: #FFC108;
}
.load-more{
	text-align: center;
	color: #AAAAAA;
	padding: 10upx;
}


/* .scroll-view-wrap {
	display: flex;	
	height: 40px;
	align-items: center;
} */
#scroll-view-classfication{
	width: 100%;
	white-space: nowrap;
	color: #FFC108;
	border: none;
}
#scroll-view-classfication .class-item{
	display: inline-block;
	margin-left: 10px;
	width: 75px;
	padding: 3px 0;
	text-align: center;
	border: 1px solid #FFC108;
	border-radius: 40px;
}
#scroll-view-classfication .active {
	background-color: #FFC108;
	color: white;
}

.recommond-wrap {
	border-bottom: 1px solid #D7D7D7;
}
.top-text-wrap,.transform-list {
	display: flex;
	color: #6E6E6E;
	justify-content: space-between;
}
.top-text{
	font-size: 14px;
	
}
.more-link{
	font-size: 12px;
}
.list-item{
	text-align: center;
}
.transform-title{
	font-size: 14px;
	color: #404040;
}
</style>
