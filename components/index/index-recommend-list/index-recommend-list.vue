<template>
	<view>
		<!-- 精品推荐  父组件向子组件传值未实现-->
		<view class="section-wrap">
			<view class="top-text-wrap">
				<text class="top-text">{{sectionName}}</text>
				<navigator url="../myTransform/myTransform" class="more-link">
					全部
					<view class="uni-icon uni-icon-arrowright" style="font-size: 16px;"></view>
				</navigator>
			</view>
			<view class="section-list">  
				<view class="list-item" v-for="(item,index) in list" :key="index" :style="{width: itemWidth+'px'}">
					<image :src="item.pic" class="item-img" mode=""></image>
					<view class="item-title">{{item.title}}</view>
					<view class="item-describe">{{item.describe}}</view>
				</view>
			</view>
		</view>	
	</view>
</template>

<script>
	export default{
		props:{
			sectionName:String,
			itemWidth:Number,
			itemHeight:Number,
			list:[]
		},
		data(){
			return{
				
			}
		}
	}
</script>

<style>
	
.section-wrap {
	padding: 10px;
	border-bottom: 1px solid #D7D7D7;
}
.top-text-wrap,.section-list {
	display: flex;
	color: #6E6E6E;
	justify-content: space-between;
}
.top-text-wrap{
	margin-bottom: 5px;
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
.item-img{
	width: 100%;
	height: 100px;
}
.item-title{
	font-size: 14px;
	color: #404040;
}
.item-describe {
	font-size: 11px;
	height: 25px;
	text-overflow: ellipsis;
	overflow: hidden;
	white-space: nowrap;
}
</style>
