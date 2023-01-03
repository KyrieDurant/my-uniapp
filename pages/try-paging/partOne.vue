<template>
	<view class="content">
		<view class="one-box">
			<view class="">1父组件传过来的：{{neirong}}</view>
			<view class="">2父组件传过来的：{{old}}</view>
			<view class="zizjdzBox">
				<input class="shurk" type="text" v-model="shuzigb">
				<view class="modifyzhi" @click="xgfzj">修改</view>
			</view>
			<view class="" @click="">子组件输入的值为:{{shuzigb}}</view>
		</view>
	</view>
</template>

<script>
	export default {
		data(){
			return {
				shuzigb:'',
				sfxx:{
					name: 'kevin',
					old: 23,
					sex: 'man',
				},
			}
		},
		// props在实际开发中写对象,一般不写字符串数组
		// 指定类型中传多个类型可这样写:propA: [String, Number],
		// props:['neirong','old'],
		props:{
			neirong: null,//指定类型（null 和 undefined 值会通过任何类型验证）
			old:{
				type:Number,//传值的类型规定
				// required:true,//是否为必传
			},
		},
		onLoad() {
			
		},
		methods:{
			xgfzj(){
				// console.log('修改父组件',this.sfxx);
				this.$emit('changeNumb',this.sfxx);
			},
		},
	}
</script>

<style >
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
.one-box{
	width: 100%;
	height: 300rpx;
	display: flex;
	flex-direction: column;
	border: 1rpx solid #ccc;
	justify-content: center;
	align-items: center;
	/* background-color: #ffaa00; */
}
.shurk{
	width: 300rpx;
	height: 70rpx;
	border: 1rpx solid #ccc;
}
.zizjdzBox{
	display: flex;
	justify-content: center;
	align-items: center;
}
.modifyzhi{
	margin-left: 20rpx;
	display: flex;
	justify-content: center;
	align-items: center;
	width: 100rpx;
	height: 70rpx;
	background-color: #65cc97;
}
	
</style>