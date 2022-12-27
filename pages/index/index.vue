<template>
	<view class="content">
		<!-- 视频 -->
		<view class="page-section">
			<video ref="myVideo" @play="quanpingVideo()" :src="src" id="myVideo" controls></video>
		</view>
		<!-- 图片 -->
		<view class="imageBox">
			<image :src="image" @tap="preImage(image)" mode="aspectFit"></image>
		</view>
		<!-- 跳转页面 -->
		<view class="butPage1" @click="getApage()">{{jumpPage}}</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				jumpPage:'跳转页面',
				image: '../../static/image/wx.jpg',
				src:'http://clips.vorwaerts-gmbh.de/big_buck_bunny.mp4',
			}
		},
		methods: {
			//点击播放自动全屏
			quanpingVideo(){
				this.videoContext = uni.createVideoContext('myVideo');//定义视频播放
				console.log('开始播放');
				this.videoContext.requestFullScreen();//进行全屏
			},
			//点击图片进行全屏预览
			preImage(image) {
				//定义存放图片的数组
				var imgArr = [];
				//把图片放入定义好的数组里
				imgArr.push(image);
				//预览图片
				uni.previewImage({
					urls: imgArr,//查看图片的数组
					current:imgArr[0],//查看数组里第1张照片
				});
			},
			//跳转页面
			getApage(){
				uni.navigateTo({
					url:'/pages/transmitPage/pageA',
				})
			},
		}
	}
</script>

<style>
	.content{
		display:flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}
	.page-section{
		margin-top: 50rpx;
	}
	.butPage1{
		display: flex;
		justify-content: center;
		align-items: center;
		margin-top: 70rpx;
		width: 300rpx;
		height: 75rpx;
		background-color: rgb(0, 255, 189);
		border: 3rpx solid rgb(0, 179, 134);
		border-radius: 20rpx;
		color: #fff;
		font-weight: 600;
	}
	.imageBox{
		display: flex;
		justify-content: center;
		align-items: center;
		width: 400rpx;
		height: 400rpx;
		
	}
</style>
