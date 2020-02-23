<template>
	<view class="body">
		<!-- 第三方登录上面的部分 -->
		<view class="header" style="height: 300px;">
			<view class="topbar">
					<view class="topbar-item" v-for="(item,index) in list" :key="index" :class="{'active':tabIndex==index}" @tap="taptab(index)">{{item}}</view>
			</view>
			<!-- 密码登录与验证码登录的模块切换 -->
			<template v-if="tabIndex==0">
				<!-- 密码登录-->
				<view class="input-wrap"> 
					<view class="input-left">+86</view>
					<view class="input-right">
						<view class="account">
							<input type="number" name="phoneNum" placeholder="请输入手机号" placeholder-class="placeholder-style" v-model="phoneNum"/>
						</view>
						<view class="password">
							<input type="password" name="password" placeholder="请输入密码" placeholder-class="placeholder-style" v-model="password"/>
						</view>
					</view>
				</view>
				<view class="register-wrap">
					<view class="go-register" @click="goRegister">注册</view>
					<view class="forgot-pass">忘记密码</view>
				</view>
			</template>
			<!-- 验证码登录 -->
			<template v-else> 
				<view class="input-wrap">
					<view class="input-left">+86</view>
					<view class="input-right">
						<view class="account">
							<input type="number" id="phoneNum" name="phoneNum" placeholder="请输入手机号" placeholder-class="placeholder-style" v-model="phoneNum"/>
						</view>
						<view class="password">
							<input type="number" id="password" name="password" placeholder="请输入验证码" placeholder-class="placeholder-style" v-model="checkNum"/>
							<view class="send-code" @click="sendCode" v-if="!isShow">获取验证码</view>
							<view  class="code-time" v-if="isShow">{{num}} 秒后重试</view>
						</view>
					</view>
				</view>
				<view class="register-wrap">
					<view class="go-register" @click="goRegister">注册</view>
				</view>
			</template>
			<view class="login-wrap">
				<button class="login" @click="login" :class="{'login-disable':disabled}" type="primary" :disabled="disabled">登录</button>
			</view>
			<view class="look" @click="goIndex">随便看看</view>
		</view>
		<!-- 底部第三方登陆模块 -->
		<view class="footer">
			<view class="other-login-way">
				<view class="sep-line">
					<view class="line"></view>
					<view class="other-login-text">第三方登录</view>
					<view class="line"></view>
				</view>
				<view class="other-login-icon-wrap">
					<view class="other-login-icon">
						<view class="login-item">
							<view class="iconfont icon-qq icon-size" @tap="qqLogin"></view>
							<view>qq登录</view>
						</view>
						<view class="login-item">
							<view class="iconfont icon-weixin icon-size" @tap="weixinLogin"></view>
							<view>微信登录</view>
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tabIndex:0,
				num:Number,//发送验证码倒计时
				isShow:false,//false代表没有点击发送验证码,true代表点击发送验证码
				list:["密码登录","验证码登录"],
				disabled:true,//登录按钮是否有效
				phoneNum:"",
				password:"",
				checkNum:""
			}
		},
		onLoad() {
			uni.getSystemInfo({
				success:(res)=> {
					console.log(res);
					let _self = this;
					//怎么获取除导航栏之外的距离？
					let info = uni.createSelectorQuery().select(".uni-tab-bar");
			　　　  		info.boundingClientRect(function(data) { //data - 各种参数
						//data.hieght为整个屏幕的高度\
						console.log(data);
			　　    }).exec();
				}
			})
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
			//初始化表单
			initInput(){
				this.phoneNum="";
				this.password="";
				this.checkNum="";
			},
			//改变按钮状态
			onBtnChange(){
				if((this.phoneNum && this.password) || (this.phoneNum && this.checkNum)){
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
			goIndex(){
				uni.navigateTo({
					url:"../index/index",
				});
			},
			goRegister(){
				uni.navigateTo({
					url:"../login/login"
				});
			},
			//切换登录选项
			taptab(index){
				this.initInput();
				this.tabIndex = index;
			},
			qqLogin:function() {
				uni.login({
					provider: "qq",					success: (resp) => {
						// var access_token=resp.authResult.access_token;
						uni.getUserInfo({
						    provider: 'qq',
						    success: function (infoRes) {
								console.log(infoRes);
								// var formdata={
								// 	nickName:infoRes.userInfo.nickname,
								// 	gender:infoRes.userInfo.gender=='男'?0:1,
								// 	openId:infoRes.userInfo.openid,
								// 	access_token:access_token,
								// };
								// console.log(formdata);
								// self.$go.post("/qqlogin",formdata).then(res=>{});
							}
						})
					},
					fail: (err) => {}
				});
			},
			weixinLogin: function(e) {
				var self=this;
				uni.login({
					provider: "weixin",
					success: (res) => {
						uni.getUserInfo({
						    provider: 'weixin',
						    success: function (infoRes) {
								console.log(infoRes);
								let formdata={
									nickName:infoRes.userInfo.nickName,
									gender:infoRes.userInfo.gender,
									openId:infoRes.userInfo.openId,
									unionId:infoRes.userInfo.unionId
								};
								console.log(formdata);
								// self.$go.post("/wxlogin",formdata).then(res=>{});
							}
						})
					},
					fail: (err) => {}
				});
			},
			submit(){
				console.log("登录.....");
			}
		}
	}
</script>

<style>
.body {
	display: flex;
	flex-direction: column;
	justify-content: center;
}
.header {
	padding: 35px 40px;
	/* height: 70%; */
}
.password-login {
	float: right;
}
.topbar {
	display: flex;
	justify-content: space-between;
	align-items: center;
	margin-bottom: 20px;
}
.topbar-item{
	font-size: 20px;
	color: #737373;
}
.active{
	color: #FF8920;
	border-bottom: 2px solid #F6610A;
}


.input-wrap {
	display: flex;
	flex-direction: row;
}
.input-left{
	padding-top: 8px;
	padding-right: 10px;
	color: #737373;
	
}
.input-right {
	display: flex;
	width: 100%;
	flex-direction: column;
}
.account,.password {
	padding: 8px 0 8px 10px;
	color: #333333;
	border-bottom: 1px solid #BBBBBB;
}
.password{
	display: flex;
	flex-direction: row;
	justify-content: space-between;
}
.account input,.password input {
	font-size: 16px;
}
.placeholder-style,.code-time {
	color: #999999;
}
.input-left,.go-register,.forgot-pass,.login,.placeholder-style,.code-time,.send-code{
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
.register-wrap {
	display: flex;
	justify-content: space-between;
	align-items: center;
	padding: 20px 0;
}
.go-register{
	color: #8192FF;
}
.forgot-pass{
	color: #999999;
}
.login-wrap{
	display: flex;
	justify-content: center;
	padding: 20px 0;
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
.look{
	text-align: center;
	color: #FFA000;
}

.footer {
	height: 30%;
}
.other-login-way {
	
}
.sep-line {
	display: flex;
	align-items: center;
	justify-content: space-between;
}
.icon-size {
	font-size: 30px;
}
.other-login-icon-wrap {
	display: flex;
	flex-direction: row;
	justify-content: center;
	color: #737373;
}
.other-login-icon{
	width: 170px;
	display: flex;
	justify-content: space-between;
	padding-top: 10px;
}
.line {
	height: 1px;
	width: 90px;
	background-color: #797979;
}
.other-login-text {
	font-size: 14px;
	color: #919191;
}
.login-item {
	display: flex;
	flex-direction: column;
}
</style>
