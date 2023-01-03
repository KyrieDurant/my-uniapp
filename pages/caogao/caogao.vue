<template>
	<view class="content">
		<view class="">
			第一部分
		</view>
		<view class="">
			第二部分
		</view>
		<view class="">
			第三部分
		</view>
	</view>
</template>

<script>
	
	export default {
		data(){
			return {
				
			}
		},
		components: {
			oneBox,
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

</style>