<template>
	<view class="content">
		<!-- 背景图片 -->
		<view class="bgVague">
			<image style="width: 100%; height: 100%; filter: blur(20rpx);background-size: cover;"
				:src="backgImg" mode=""></image>
		</view>
		<!-- 页面内容 -->
		<view class="pageCont">
			<!-- 页面上方内容 -->
			<view class="pageTop">
				<view class="">{{musicName}}</view>
				<view class="">歌手：{{singEr}}</view>
			</view>
			<!-- 页面中间部分 -->
			<view class="pageCenter">
				<!-- 放大线 -->
				<view class="enlargeLine1"></view>
				<view class="enlargeLine2"></view>
				<view class="enlargeLine3"></view>
				<!-- 圆盘 -->
				<!-- style="animationPlayState: paused;"   class="platform" -->
				
				<view ref="kzhiyp" :class="diskCssys" :style="platform" >
					<image :src="diskImg" mode="aspectFit"
					 style="width: 100%; height: 100%;"></image>
				</view>

			</view>
			<!-- 页面底部 -->
			<view class="PageBottom">
				<!-- 上面小图标 -->
				<view class="smxtb">
					<view class="smxtb1" v-for="(item,index) in tbList" :key="index">
						<view class="smxtbPic" @click="goBut(item.funct)">
							<image :src="item.img" mode="aspectFit" style="width: 100%; height: 100%;"></image>
						</view>
					</view>
				</view>
				<!-- 中间滑动选择器 -->
				<view class="playhd">
					<!-- 开始时间 -->
					<view class="kstime">{{ksTime}}</view>
					<!-- 进度条 -->
					<view class="jdtime">
						<slider @change="sliderChange" @changing="tdgcz" :min="0" :max="maxJdtbar"
						block-size="10" activeColor="#8bb8ff"/>
					</view>
					<!-- 结尾时间 -->
					<view class="jstime">{{zbfTime}}</view>
				</view>
				<!-- 下面操作键 -->
				<view class="xmBtn">
					<!-- 播放模式 -->
					<view class="xm-item">
						<view class="xmanPic" @click="goXdms()">
							<image :src="playMode"
							 mode="aspectFit" style="width: 100%; height: 100%;"></image>
						</view>
					</view>
					<!-- 上一曲 -->
					<view class="xm-item">
						<view class="xmanPic" @click="shangyqu()">
							<image src="../../../static/image/wyy/syq.png"
							 mode="aspectFit" style="width: 100%; height: 100%;"></image>
						</view>
					</view>
					<!-- 暂停/播放 -->
					<view class="xm-item2">
						<view class="xmanPic2" @click="playMusic()">
							<image :src="bfOrzt"
							 mode="aspectFit" style="width: 100%; height: 100%;"></image>
						</view>
					</view>
					<!-- 下一曲 -->
					<view class="xm-item">
						<view class="xmanPic" @click="xiayqu()">
							<image src="../../../static/image/wyy/xyq.png"
							 mode="aspectFit" style="width: 100%; height: 100%;"></image>
						</view>
					</view>
					<!-- 音乐列表 -->
					<view class="xm-item">
						<view class="xmanPic" @click="musicList()">
							<image src="../../../static/image/wyy/yylb.png"
							 mode="aspectFit" style="width: 100%; height: 100%;"></image>
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
				//背景图片
				backgImg:'../../../static/image/xzPic/winter-four.jpg',
				//圆盘图片
				diskImg:'../../../static/image/xzPic/winter-one.jpg',
				//圆盘css样式
				diskCssys:'platform1',
				//圆盘转动状态
				platform:{
					animationPlayState:'paused',
					animationDelay:'0.6s',
				},
				
				// 喜欢下载音质评论更多
				tbList: [{
						img: '../../../static/image/wyy/axwd.png',
						funct: 1,
					},
					{
						img: '../../../static/image/wyy/xz.png',
						funct: 2,
					},
					{
						img: '../../../static/image/wyy/ld.png',
						funct: 3,
					},
					{
						img: '../../../static/image/wyy/pl.png',
						funct: 4,
					},
					{
						img: '../../../static/image/wyy/gd.png',
						funct: 5,
					},
				],
				//音乐列表
				musicLists:[
					{
						id: 1,
						name:'追光者',//歌名
						singer:'岑宁儿',//歌手
						murl:'https://www.ytmp3.cn/down/55480.mp3',//音乐链接
						backgImg:'../../../static/image/xzPic/winter-four.jpg',//背景图片
						diskImg:'../../../static/image/xzPic/winter-one.jpg',//圆盘图片
					},
					{
						id: 2,
						name:'钢琴曲',
						singer:'莫扎特',
						murl:'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-hello-uniapp/2cc220e0-c27a-11ea-9dfb-6da8e309e0d8.mp3',
						backgImg:'../../../static/image/xzPic/winter-three.jpg',//背景图片
						diskImg:'../../../static/image/xzPic/winter-two.jpg',//圆盘图片
					},
				],
				muId: 0,
				musicName:'Music Name',//当前音乐名称
				singEr:'',//当前歌手
				musicUrl:'',//当前音乐播放链接
				
				playMode:"../../../static/image/wyy/xdms.png",// 播放模式
				bfOrzt:"../../../static/image/wyy/bf.png",//播放/暂停
				maxJdtbar: 100,//滑动进度条最大值
				ksTime:'00:00',//开始播放时间
				zbfTime:'00:00',//总播放时间
				
				
			}
		},
		onLoad() {
			this.tdjdts = false;//拖动进度条
			this.wctdjdt = null;//完成拖动进度条后
			this.getmusicInfo();//加载默认音乐
		},
		methods: {
			//初始化加载
			getmusicInfo(){
				console.log('初始化加载');
				// this.muId = this.musicLists[0].id;
				this.musicName = this.musicLists[0].name;
				this.singEr = this.musicLists[0].singer;
				this.musicUrl = this.musicLists[0].murl;
				this.backgImg = this.musicLists[0].backgImg;
				this.diskImg = this.musicLists[0].diskImg;
				this.obtainMusic();
			},
			// 加载音乐
			obtainMusic(){
				var innerAudioContext = this._audioContext = uni.createInnerAudioContext();
				innerAudioContext.autoplay = false;//是否自动开始播放
				innerAudioContext.src = this.musicLists[0].murl;//播放链接
				console.log('音频播放文件',innerAudioContext.src);
				//音频可以进入播放状态
				innerAudioContext.onCanplay(()=>{
					// 定义timeid定时器
					let timeid = setInterval(()=>{
						if(innerAudioContext.duration){
							//销毁定时器
							clearInterval(timeid);
							//给滑块最大值赋值
							this.maxJdtbar = innerAudioContext.duration;
							console.log('进度条最大值',this.maxJdtbar);
							//转换播放时长
							this.zbfTime = formatSecond(this.maxJdtbar);
						}
					},500)
				});
			},
			// 进度条上方功能
			goBut(row) {
				if (row == 1) {
					this.likeit();//喜欢音乐
				} else if (row == 2) {
					this.mudownLoad();//下载音乐
				}else if (row == 3) {
					this.upMuyinzhi();//提升音质
				} else if (row == 4) {
					this.goPinglun();//点击评论
				} else if (row == 5) {
					this.moreGn();//更多功能
				}
			},
			// 喜欢音乐
			likeit() {
				console.log('点击爱心');
				this.tbList[0].img = '../../../static/image/wyy/axdj.png';
				uni.showToast({
					icon:'none',
					title: '已添加到喜欢的音乐',
					duration: 2000
				});

			},
			// 下载音乐
			mudownLoad() {
				console.log('点击下载音乐');
				uni.showToast({
					icon:'none',
					title: '音乐下载成功',
					duration: 2000
				});
			},
			//提升音质
			upMuyinzhi(){
				console.log('点击提升音质');
			},
			//点击评论
			goPinglun(){
				console.log('点击评论');
			},
			//更多功能
			moreGn(){
				console.log('点击更多功能');
			},
			//拖动进度条时
			tdgcz(){
				
			},
			// 拖动进度条完成后
			sliderChange(e) {
				console.log('value 发生变化：', e.detail.value);
				
			},
			//播放模式
			goXdms(){
				console.log('切换播放模式');
				if(this.playMode == "../../../static/image/wyy/xdms.png"){
					console.log('随机播放')
					this.playMode = "../../../static/image/wyy/sxbf.png";
				} else if(this.playMode == "../../../static/image/wyy/sxbf.png"){
					console.log('顺序播放')
					this.playMode = "../../../static/image/wyy/sjbf.png";
				} else if(this.playMode == "../../../static/image/wyy/sjbf.png"){
					console.log('单曲循环')
					this.playMode = "../../../static/image/wyy/dqxh.png";
				} else if(this.playMode == "../../../static/image/wyy/dqxh.png"){
					console.log('心动模式')
					this.playMode = "../../../static/image/wyy/xdms.png";
				}
				
			},
			// 上一曲
			shangyqu(){
				if(this.diskCssys == 'platform1'){
					this.diskCssys = 'platform2';
				} else if(this.diskCssys = 'platform2'){
					this.diskCssys = 'platform1';
				}
				//切换
				const syqid = this.muId-1;
				console.log('上一曲的id',syqid);
				this.musicName = this.musicLists[syqid].name;
				this.singEr = this.musicLists[syqid].singer;
				this.musicUrl = this.musicLists[syqid].murl;
				this.backgImg = this.musicLists[syqid].backgImg;
				this.diskImg = this.musicLists[syqid].diskImg;
				
			},
			// 播放/暂停
			playMusic(){
				// console.log('点击播放音乐');
				if(this.bfOrzt=="../../../static/image/wyy/bf.png"){
					// 已播放
					console.log('点击播放');
					this.bfOrzt="../../../static/image/wyy/zt.png";
					this.platform.animationPlayState = 'running';
				} else if(this.bfOrzt=="../../../static/image/wyy/zt.png"){
					// 已暂停
					console.log('点击暂停');
					this.bfOrzt="../../../static/image/wyy/bf.png";
					this.platform.animationPlayState = 'paused';
				}
			},
			// 下一曲
			xiayqu(){
				if(this.diskCssys == 'platform1'){
					this.diskCssys = 'platform2';
				} else if(this.diskCssys = 'platform2'){
					this.diskCssys = 'platform1';
				}
				//切换
				console.log('初始aaa', this.muId);
				const xyqid = this.muId + 1;
				this.muId = this.muId + 1;
				console.log('下一曲的id',xyqid);
				console.log('音乐列表长度',this.musicLists.length);
				if(this.muId < this.musicLists.length){
					this.musicName = this.musicLists[xyqid].name;
					this.singEr = this.musicLists[xyqid].singer;
					this.musicUrl = this.musicLists[xyqid].murl;
					this.backgImg = this.musicLists[xyqid].backgImg;
					this.diskImg = this.musicLists[xyqid].diskImg;
				} else if(this.muId > this.musicLists.length || this.muId == this.musicLists.length) {
					this.musicName = this.musicLists[0].name;
					this.singEr = this.musicLists[0].singer;
					this.musicUrl = this.musicLists[0].murl;
					this.backgImg = this.musicLists[0].backgImg;
					this.diskImg = this.musicLists[0].diskImg;
					this.muId = 0;
				}
				
				
			},
			musicList(){
				console.log('音乐列表');
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
			

		},
	}
</script>

<style>
	@import url("wyy-playcover.css");
</style>
