<template>
	<view class="conter">
		<view class="openPop">
			<button @click="getAudio()">点击播放音乐</button>
		</view>
		
		<!-- 音频播放弹窗 -->
		<view :hidden="musicPopwin" class="popup_content">
			<view @click="closewinPop()" class="returnPop">
				<view class="tuiIcon">×
					<!-- <uni-icons type="clear" size="30"></uni-icons> -->
				</view>
			</view>
			<view class="audio-view">
				<!-- 播放/暂停 -->
				<view class="play-button-area">
					<image class="icon-play" :src="playImage" @click="play"></image>
				</view>
				<view class="play-silder">
					<view class="audio-time">
						<text>{{currentTimeStr}}</text>
					</view>
					<view class="audio-siler">
						<slider :value="position" :min="0" :max="duration" @changing="onchanging" @change="onchange">
						</slider>
					</view>
					<view class="audio-time">
						<text>{{timeStr}}</text>
					</view>
				</view>
			</view>
		</view>
		<view class="popup_overlay" :hidden="musicPopwin" @click="hideDiv()"></view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				musicPopwin: true, //为true时弹窗默认隐藏
				isPlaying: false,//暂停/播放音乐
				isPlayEnd: false,//进度条
				currentTime: 0,//播放时间
				currentTimeStr: '00:00',//播放到进度时间
				duration: 100,//进度条最大值
				timeStr: '00:00',//总播放时间
			}
		},
		computed: {
			//判断开始播放音乐的时间
			position() {
				return this.isPlayEnd ? 0 : this.currentTime;
			},
			// 判断播放/暂停按钮
			playImage() {
				return this.isPlaying ? "../../static/Music-icon/zanting.png" : "../../static/Music-icon/bofang.png"
			}
		},
		onLoad() {
			this._isChanging = false;//拖动进度条
			this._audioContext = null;//完成拖动进度条后
		},
		onUnload() {
			// 音频播放
			if (this._audioContext != null && this.isPlaying) {
				this.stop();
			}
		},
		methods: {
			// 点击播放音乐
			getAudio() {
				this.musicPopwin = false;//打开弹窗
				this.createAudio();
			},
			hideDiv() {
				// this.musicPopwin = true;
				//弹窗周围阴影部分
				console.log('22222');
			},
			// 关闭弹窗
			closewinPop() {
				this.musicPopwin = true;//关闭弹窗
				this._audioContext.destroy();//销毁播放进程
				this.currentTime = 0;//进度条时间为0
				this.currentTimeStr = '00:00';//播放到进度时间
				this.isPlaying = false;//暂停音乐
				this.isPlayEnd = false;//进度条
			},
			//获取音频文件
			createAudio() {
				// 音频文件
				// 'https://www.ytmp3.cn/down/55480.mp3'
				// 'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-hello-uniapp/2cc220e0-c27a-11ea-9dfb-6da8e309e0d8.mp3'
				const audioUrl = 'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-hello-uniapp/2cc220e0-c27a-11ea-9dfb-6da8e309e0d8.mp3';
				console.log('音乐',audioUrl);
				var innerAudioContext = this._audioContext = uni.createInnerAudioContext();
				innerAudioContext.autoplay = false;//是否自动开始播放
				innerAudioContext.src = audioUrl;//播放链接
				//音频可以进入播放状态
				innerAudioContext.onCanplay(() => {
					// 定义timeid定时器
					let timeid = setInterval(() => {
						if (innerAudioContext.duration) {
							//销毁定时器
							clearInterval(timeid);
							this.duration = innerAudioContext.duration || 0;
							console.log('音频时长',this.duration);
							console.log(this.duration)
							this.timeStr = this.formatSecond(this.duration);
						}
					}, 500)
				});
				//拖动播放进度条
				innerAudioContext.onTimeUpdate((e) => {
					if (this._isChanging === true) {
						//拖动进度条过程中
						return;
					}
					this.currentTime = innerAudioContext.currentTime || 0;
					// console.log('音频播放位置',this.currentTime);
					this.duration = innerAudioContext.duration || 0;
					// console.log('更新进度条的值',this.duration);
					this.currentTimeStr = this.formatTime(this.currentTime);
					// console.log('更新已播放的时间位置',this.currentTimeStr);
				});
				//自然播放完成后
				innerAudioContext.onEnded(() => {
					this.currentTime = 0;
					this.currentTimeStr = this.formatTime(this.currentTime);
					this.isPlaying = false;
					this.isPlayEnd = true;
				});
				// 音频播放错误事件
				innerAudioContext.onError((res) => {
					this.isPlaying = false;
					console.log(res.errMsg);
					console.log(res.errCode);
				});
				return innerAudioContext;
			},
			// 拖动进度条时
			onchanging() {
				this._isChanging = true;
				console.log('aaa',this._isChanging);
			},
			// 拖动进度条完成后
			onchange(e) {
				console.log('进度条的数字', e.detail.value);
				console.log('进度条的类型',typeof e.detail.value);
				this._audioContext.seek(e.detail.value);
				this._isChanging = false;
				const currTimeStr = this.formatTime(e.detail.value)
				this.currentTimeStr = currTimeStr
			},
			//播放
			play() {
				if (this.isPlaying) {
					this.pause();
					return;
				}
				this.isPlaying = true;
				this._audioContext.play();
				this.isPlayEnd = false;
			},
			//暂停
			pause() {
				this._audioContext.pause();
				this.isPlaying = false;
			},
			// 完成播放后
			stop() {
				console.log('ddd------ddddd');
				this._audioContext.stop();
				this.isPlaying = false;
			},
			// 格式化时间
			formatTime(num) {
				//格式化时间格式
				num = num.toFixed(0);
				let second = num % 60;
				if (second < 10) second = '0' + second;
				let min = Math.floor(num / 60);
				if (min < 10) min = '0' + min;
				return min + ":" + second;
			},
			/**
			 * 格式化时间 
			 * @param {String} date 原始时间格式
			 * 格式后的时间：hh:mm:ss
			 **/
			formatSecond(seconds) {
				var h = Math.floor(seconds / 3600) < 10 ? '0' + Math.floor(seconds / 3600) : Math.floor(seconds / 3600);
				var m = Math.floor((seconds / 60 % 60)) < 10 ? '0' + Math.floor((seconds / 60 % 60)) : Math.floor((
					seconds / 60 % 60));
				var s = Math.floor((seconds % 60)) < 10 ? '0' + Math.floor((seconds % 60)) : Math.floor((seconds % 60));
				return m + ":" + s;
			},
			
			
		}
	}
</script>

<style>
	.conter{
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}
	.openPop{
		margin-top: 100rpx;
	}
	
	.popup_overlay {
		position: fixed;
		top: 0%;
		left: 0%;
		width: 100%;
		height: 100%;
		/* background-color: black; */
		background-color: rgb(0, 0, 0,0.6);
		z-index: 1001;
		/* -moz-opacity: 0.8;
		opacity: .60;
		filter: alpha(opacity=88); */
	}

	.popup_content {
		position: fixed;
		top: 30%;
		left: 0;
		right: 0;
		width: 90%;
		height: 150rpx;
		margin: 0 auto;
		background-color: white;
		z-index: 1002;
		overflow: auto;
	}

	.popup_title {
		width: 480rpx;
		text-align: center;
		font-size: 32rpx;
	}

	.popup_textarea {
		width: 410rpx;
		font-size: 26rpx;
		margin-left: 20rpx;
	}

	.popup_button {
		color: #000000;
	}

	.returnPop{
		height: 50rpx;
		width: 100%;
		display: flex;
		flex-direction: row-reverse;
	}
	
	.tuiIcon {
		width: 50rpx;
		height: 50rpx;
		display: flex;
		justify-content: center;
		align-items: center;
		font-size: 60rpx;
		color: #fff;
		background-color: #007AFF;
	}
	
	
	.audio-view {
		display: flex;
		height: 100rpx;
		justify-content: space-between;
		border: 1rpx #dadada solid;
		padding: 0rpx 20rpx;
		/* background-color: rgb(102, 255, 217, 0.2); */
	}
	
	.play-time-area {
		display: flex;
		flex-direction: row;
	}
	
	.audio-time {
		width: 150rpx;
		line-height: 100rpx;
	}
	
	.duration {
		margin-left: auto;
	}
	
	.audio-siler {
		width: 100%;
	}
	
	.play-silder {
		width: 100%;
		display: flex;
		justify-content: space-between;
		align-items: center;
	}
	
	.play-button-area {
		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: center;
		width: 100rpx;
	}
	
	.icon-play {
		width: 50rpx;
		height: 50rpx;
		/* margin-top: 5rpx; */
		/* background-color: #ffcccc; */
	}
</style>
