<template>
	<view>
		<view class="new-voice-wrap">
			<view class="iconfont icon-shangchuan"></view>
			<view class="new-voice-test">录入新声音</view>
		</view>
		<view class="voice-list-wrap">
			<view class="voice-list">
				<view class="voice-name">1、爸爸声音</view>
				<view class="tap-tab">一键切换</view>
			</view>
			<view class="voice-list">
				<view class="voice-name">1、妈妈声音</view>
				<view class="tap-tab">一键切换</view>
			</view>
			<view class="voice-list">
				<view class="voice-name">1、姐姐声音</view>
				<view class="tap-tab">一键切换</view>
			</view>	
		</view>
		<view>
			<button @tap="startRecord">开始录音</button>
			<button @tap="endRecord">停止录音</button>
			<button @tap="playVoice">播放录音</button>
		</view>
	</view>
</template>

<script>
	const recorderManager = uni.getRecorderManager();
		const innerAudioContext = uni.createInnerAudioContext();
		//内置播放器
		innerAudioContext.autoplay = true;
	
		export default {
			data() {
				return {
					indicatorDots: true,
					autoplay: true,
					interval: 2000,
					duration: 500
				}
			},
			onLoad() {
				let self = this;
				recorderManager.onStop(function (res) {
					console.log('recorder stop' + JSON.stringify(res));
					self.voicePath = res.tempFilePath;
				});
			},
			methods: {
				startRecord() {
					console.log('开始录音');
		
					recorderManager.start();
				},
				endRecord() {
					console.log('录音结束');
					recorderManager.stop();
				},
				playVoice() {
					console.log('播放录音');
		
					if (this.voicePath) {
						//获取到当前录音的暂时存储路径
						innerAudioContext.src = this.voicePath;
						innerAudioContext.play();
					}
				}
		},
	}
</script>

<style>
page{
	background-color: #EEEEEE;
}
.new-voice-wrap {
	background-color: #FFFFFF;
	display: flex;
	align-items: center;
	height: 80px;
	margin: 20px 0;
}
.icon-shangchuan {
	color: #FF8920;
}
.new-voice-test{
	font-size: 18px;
	color: #6A6A6A;
}
.voice-list-wrap{
	background-color: #FFFFFF;
	padding-bottom: 20px;
}
.voice-list {
	height: 40px;
	display: flex;
	align-items: center;
	margin: 0 35px;
	justify-content: space-between;
	border-bottom: 1px solid #D4D4D4;
}
.voice-name {
	font-size: 14px;
}
.tap-tab {
	font-size: 12px;
	background-color: #FFA000;
	border-radius: 6px;
	color: #FFFFFF;
	padding: 0 10px;
}
</style>
