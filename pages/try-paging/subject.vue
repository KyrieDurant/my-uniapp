<template>
	<view class="content">
		<view class="benwj">
			<input type="text" class="fzjsrk" v-model="yigez">
			<view class="zizj">子组件传过来：{{childUp}}</view>
		</view>
		<view class="diyibf">
			<!-- 直接传对象 -->
			<!-- <one-box :neirong="yigez"></one-box> -->
			<!-- 传对象里的值 -->
			<!-- <one-box :neirong="message.name"></one-box> -->
			<!-- v-bind传到子组件 -->
			<!-- v-bind直接传了message里面的所有，在子组件里拿也只需要message里面有 -->
			<one-box v-bind="message" @changeNumb="chaNum"></one-box>
		</view>
	</view>
</template>

<script>
	import oneBox from './partOne.vue';
	
	export default {
		data(){
			return {
				xszhi:'',
				yigez:'这是父组件的内容',
				message:{
					name:'滴滴',
					old:18,
					// neirong:'数组里的父组件内容',
				},
				childUp:'',
				
			}
		},
		components: {
			'one-box':oneBox,
		},
		onLoad() {
			
		},
		methods:{
			chaNum(e){
				console.log('拿到子组件传过来的', e);
				// this.childUp = e;
			},
		},
	}
</script>

<style scoped>
page{
	width: 100%;
	height: 100%;
}
.content{
	width: 100%;
	height: 100%;
	display: flex;
	flex-direction: column;
}
.fzjsrk{
	border: 1rpx solid #ccc;
	height: 70rpx;
	width: 300rpx;
}
.zizj{
	height: 100rpx;
	display: flex;
	justify-content: center;
	align-items: center;
	/* background-color: #55aa7f; */
}
.benwj{
	width: 100%;
	height: 300rpx;
	display: flex;
	flex-direction: column;
	border: 1rpx solid #ccc;
	justify-content: space-around;
	align-items: center;
	/* background-color: #41aa90; */
}
.diyibf{
	margin-top: 20rpx;
}

</style>