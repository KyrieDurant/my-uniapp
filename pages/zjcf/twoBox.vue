<template>
  <view class="content">
    <view class="twoBox">
        <view class="">name的值:{{name}}</view>
		<view class="">number的值:{{number}}</view>
    </view>
  </view>
</template>

<script>
  export default {
    data(){
		return {
			
		}
	},
	props:{
		name:null,
		number:{
			type: Number,
		},
		
	}
	
  }
</script>

<style scoped>
page{
	width:100%;
	height:100%;
}
.content{
	width:100%;
	height:100%;
	display:flex;
	flex-direction: column;
}
.twoBox{
	width: 100%;
	height: 200rpx;
	background-color: #cbffca;
}
</style>