<template>
	<view class="content">
		<view class="faCont">
			<view class="faShurBox">
				<view class="">父组件输入</view>
				<input class="fainp" type="text" v-model="fzjsr">
			</view>
		</view>
		<view>
			<one-box :shurneir="fzjsr"></one-box>
		</view>
		<view>
			<two-box v-bind="info"></two-box>
		</view>
		<view>
			<three-box @nadaoz="getChild"></three-box>
		</view>
	</view>
</template>

<script>
	import oneBox from './oneBox.vue'
	import twoBox from './twoBox.vue'
	import threeBox from './threeBox.vue'
	
	export default {
		data(){
			return {
				fzjsr:'',
				info:{
						name:'kevin',
						number: 7,
					},
					
			}
		},
		components: {
			oneBox,
			twoBox,
			threeBox,
		},
		onLoad() {
			
		},
		methods:{
			getChild(e){
				console.log('父组件拿到值', e);
			},
		},
	}
</script>

<style >
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
.faCont{
	width: 100%;
	height: 250rpx;
	background-color: #ff9a8c;
	display: flex;
	justify-content: center;
}
.faShurBox{
	width: 100%;
	height: 100rpx;
	display: flex;
	justify-content: center;
	align-items: center;
}
.fainp{
	margin-left: 10rpx;
	background-color: #fff;
}
</style>