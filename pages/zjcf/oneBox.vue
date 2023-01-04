<template>
  <view class="content">
    <view class="oneBox">
        <view class="zizujxs">子组件显示：{{shurneir}}</view>
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
		shurneir:null,
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
.oneBox{
	width: 100%;
	height: 100rpx;
	background-color: #c5cbff;
}
.zizujxs{
	width: 100%;
	height: 80rpx;
	/* background-color: #fff; */
	display: flex;
	justify-content: center;
	align-items: center;
}
</style>