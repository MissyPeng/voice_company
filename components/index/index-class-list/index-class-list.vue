<template>
	<view>
		<view class="class-list" :item=item :index=index>
			<view class="class-list-item">
				<image :src="imgUrl" mode="" style="width: 250upx;height: 250upx;"></image>
				<view class="item-info">
					<view class="item-intro">
						<view class="item-title">
							{{item.title}}
							小白兔与大灰狼
							</view>
							<view>{{item.pic}}</view>
						<view class="item-describe">{{item.describe}}XXXXXXXXXX</view>
					</view>
					<view>{{item.releaseTime}}2020.2.18上新 </view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		props: {
			"item":Object,
			"index":Number
		},
		data() {
			return {
				// item:Object,
				imgUrl:""
			};
		},
		onLoad(option) {
			//不执行？，为实现图片src动态绑定
			console.log(option);
			this.imgUrl="../../../static/balloon.jpg"; 
			// this.imgUrl = require(item.pic);
		}
	}
</script>

<style scoped>  
.class-list {
	padding: 25upx 20upx;
	border-bottom: 1px solid #D7D7D7;
	color: #555555;
	font-size: 34upx;
}
.class-list-item {
	display: flex;
	
}
.item-info {
	display: inline-flex;
	flex-direction: column;
	justify-content: space-between;
	padding-left: 100upx;
}
.item-title {
	font-size: 34upx;
}
.item-describe {
	font-size: 30upx;
}
</style>
