<template>
	<view>
		<!-- 登录状态下 -->
		<view class="my-info-wrap" v-if="isLogin">
			<view class="my-icon">
				<image src="../../static/bear.jpg" class="img"></image>
			</view>
			<view class="my-info">
				<view class="nick-name">tom</view>
				<view class="">不知道这是啥区域</view>
			</view>
		</view>
		<!-- 未登录状态下 -->
		<view class="my-info-wrap" v-if="!isLogin">
			<view class="my-icon">
				<view class="iconfont icon-denglu"></view>
			</view>
			<view class="my-info">
				<view class="go-login" @tap="login">点击登录</view>
			</view>
		</view>
		<!-- 下面的列表选项 -->
		<view class="my-list">
			<view class="list-item my-voice">
				<text class="iconfont icon-lingdang1 list-icon"></text>
				<text>我的声音</text>
			</view>
			<view class="list-item">
				<text class="iconfont icon-shijian list-icon"></text>
				<text>历史记录</text>
			</view>
			<view class="list-item">
				<text class="iconfont icon-xihuan list-icon"></text>
				<text>我的喜欢</text>
			</view>
			<view class="list-item">
				<text class="iconfont icon-guanyu list-icon"></text>
				<text>我的故事</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				isLogin:true
			}
		},
		methods:{
			login(){
				uni.navigateTo({
					url:"../login/login"
				})
			}
		}
	}
</script>

<style>
page{
	background-color: #EEEEEE;
}
.my-info-wrap {
	display: flex;
	align-items: center;
}
.my-icon{
	width: 100px;
	height: 100px;
	display: flex;
	justify-content: center;
	align-items: center;
}
.icon-denglu{
	font-size: 50px;
	color: #BFBFBF;
}
.img{
	width: 60%;
	height: 60%;
	border-radius: 50%;
}
.my-info{
	font-size: 18px;
	color: #6C6C6C;
}
.go-login{
	font-size: 18px;
	color: #7F7F7F;
}
.nick-name {
	font-size: 18px;
}

.my-list {
	padding-top: 20px;
}
.list-item,.my-voice {
	background-color: #FFFFFF;
	display: flex;
	flex-direction: row;
	align-items: center;
	height: 40px;
	font-size: 14px;
	padding-left: 50px;
}
.list-item{
	border-bottom: 1px solid #D4D4D4;
}
.my-voice {
	margin-bottom: 20px;
}
.list-icon {
	font-size: 20px;
	color: #FF8920;
	padding-right: 15px;
}
</style>
