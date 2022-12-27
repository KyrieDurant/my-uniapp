<template>
	<view class="content">
		<view class="container">
			<!-- 轨迹 -->
			<view class="line">
				<!-- 滑动点 -->
				<view class="slideCircle"></view>
				<!-- 中心原点 -->
				<view class="cen"></view>
			</view>
		</view>
		<view :style="mryangshi" class="shiyanhhh">111</view>
	</view>
</template>

<script>
	
	export default {
		
		data() {
			return {
				mryangshi:{
					width:'200rpx',
					height:'200rpx',
					color: '#ccc',
					background: 'rgb(11, 160, 188)',
				},
			}
		},

	}
</script>

<style>
	@import url("rotateImg.css");
</style>
