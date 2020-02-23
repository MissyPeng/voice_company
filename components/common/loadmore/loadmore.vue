<template>
	<view>
		<view class="loadmore">{{loadtest}}</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				props:{
					loadtest: String
				}
			}
		},
		methods: {
			
		}
	}
</script>

<style scoped>
.loadmore{
	text-align: center;
	color: #AAAAAA;
	padding: 15upx;
}
</style>
