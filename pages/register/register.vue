<template>
	<view class="body">
		<view class="image">
			<image src="../../static/images/register.jpg" mode=""></image>
		</view>
		<view class="input-wrap">
			<view class="input-item-wrap">
				<view class="iconfont icon-denglu1 register-icon"></view>
				<view class="input-item">
					<view class="baliu">+86</view>
					<input type="number" id="phoneNum" name="phoneNum" placeholder="请输入手机号" placeholder-class="placeholder-style" v-model="phoneNum"/>
				</view>
			</view>
			<view class="input-item-wrap">
				<view class="iconfont icon-password register-icon"></view>
				<view class="input-item">
					<input type="password" name="password" placeholder="请输入密码" placeholder-class="placeholder-style" v-model="password"/>
				</view>
			</view>
			<view class="input-item-wrap">
				<view class="iconfont icon-mobile register-icon"></view>
				<view class="input-item">
					<input type="number" id="password" name="password" placeholder="请输入验证码" placeholder-class="placeholder-style" v-model="checkNum"/>
					<view class="send-code" @click="sendCode" v-if="!isShow">获取验证码</view>
					<view  class="code-time" v-if="isShow">{{num}} 秒后重试</view>
				</view>
			</view>
		</view>
		<view class="login-wrap">
			<button class="login" @click="register" :class="{'login-disable':disabled}" type="primary" :disabled="disabled">注册</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				num:Number,//发送验证码倒计时
				isShow:false,//false代表没有点击发送验证码,true代表点击发送验证码
				disabled:true,//登录按钮是否有效
				phoneNum:"",
				password:"",
				checkNum:""
			}
		},
		watch:{
			// 实时监听input框内的值,改变登录按钮的状态
			phoneNum(val){
				this.onBtnChange();
			},
			password(val){
				this.onBtnChange();
			},
			checkNum(val){
				this.onBtnChange();
			}
		},
		methods: {
			//改变按钮状态
			onBtnChange(){
				if(this.phoneNum && this.password && this.checkNum){
					this.disabled = false;
					return;
				}
				this.disabled = true;
			},
			// 验证手机号的合法性
			isPhone(){
				let myPattern = /^1[34578]\d{9}$/;
				return myPattern.test(this.phoneNum);
			},
			//发送验证码
			sendCode(){
				if(!this.isPhone()){
					uni.showToast({
						title:'请输入正确的手机号码',
						icon:'none'
					})
					return;
				}
				this.num = 60;//60秒倒计时
				this.isShow=true;
				var jian=setInterval(()=>{
					this.num--;
					if(this.num==0){
						clearInterval(jian);
						this.isShow=false;
					}
				},1000);
			},
			register(){
				uni.showToast({
					title:"登录成功",
					icon:"none"
				})
			}
		}
	}
</script>

<style>
.body{
	padding: 10px 40px;
}
.image {
	width: 100%;
}
.image image{
	width: 100%;
}
.input-wrap {
	display: flex;
	width: 100%;
	flex-direction: column;
}
.input-item-wrap {
	display: flex;
	justify-content: space-between;
	align-items: center;
}
.register-icon {
	width: 15%;
	color: #FF8920;
	font-size: 22px;
}
.input-item {
	width: 85%;
	display: flex;
	flex-direction: row;
	justify-content: space-between;
	align-items: center;
	color: #333333;
	border-bottom: 1px solid #BBBBBB;
}
.input-item input { 
	padding: 8px 0 8px 10px;
	font-size: 16px;
}
.baliu { 
	color: #737373;
}
.placeholder-style,.code-time {
	color: #999999;
}
.baliu,.login,.placeholder-style,.code-time,.send-code{
	font-size: 14px;	
} 
.code-time,.send-code {
	width: 120px;
	text-align: center;
}
.send-code {
	color: #FFFFFF;
	background-color: #FFB841;
	border-radius: 5px;
}
.login-wrap{
	display: flex;
	justify-content: center;
	padding: 45px 0;
}
.login{
	background-color: #FFA000 !important;
	padding: 2px 0;
	width: 170px;
	border-radius: 40px;
	color: #FFFFFF;
}
.login-disable {
	background-color: #999999 !important;
}
</style>
