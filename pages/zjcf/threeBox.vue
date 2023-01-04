<template>
  <view class="content">
    <view class="threeBox">
        <view class="xiaofk" @click="passValue(index)"
		 v-for="(item,index) in mindList" :key="index">
        	<view class="">{{item.name}}</view>
        	<view class="">{{item.number}}</view>
        </view>
    </view>
  </view>
</template>

<script>
  export default {
    data(){
		return {
			mindList:[
				{
					name:'kevin',
					number: 7,
				},
				{
					name:'kyrie',
					number: 11,
				},
				{
					name:'harden',
					number: 13,
				}
			]
		}
	},
	methods:{
		passValue(index){
			//点击传给父组件
			console.log('看看值',this.mindList[index]);
			this.$emit('nadaoz', this.mindList[index]);
		},
	},
	
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
.threeBox{
	width: 100%;
	height: 200rpx;
	background-color: #c1c1c1;
	display: flex;
}
.xiaofk{
	width: 200rpx;
	height: 200rpx;
	color: #fff;
	background-color: #000000;
	margin-left: 10rpx;
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
}
</style>