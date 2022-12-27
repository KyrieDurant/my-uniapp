<template>
	<view class="content">
		<view class="dataId">A页面的值为：{{id}}
			<!-- <view>请输入A页面的值</view>
			<input type="text"> -->
		</view>
		<view class="jumpBpage" @click="getBpage(id)">跳转B页面</view>
		<view class="playMusic" @click="openPop()">自定义弹窗</view>
		<view class="changecolorBox">
			<!-- css动画绕着方框走 -->
			<view class="around"></view>
			<view class="xuanzBox">
				<view class="changfx"></view>
			</view>
			
		</view>
		<view class="changecolorBox">
			<!-- 画三角形 -->
			<view class="sjxBox">
				<view class="triangle"></view>
			</view>
			<!-- 各种图形 -->
			<view class="circle"></view>
			<!-- 各种形状椭圆 -->
			<view class="tuoyuan"></view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				id: '11',

			}
		},
		onLoad() {
			this.getmergeArray(); //数组合并小案例
		},
		methods: {
			getBpage(id) {
				uni.navigateTo({
					url: '/pages/transmitPage/pageB?id=' + id,
				})
			},
			getmergeArray() {
				// 进行数组合并的方法:
				//1、运用for循环把arr2里的元素合并成
				// let arr1 = [1,2];
				// let arr2 = [3,4];
				// for( let i=0; i<arr2.length; i++){
				// 	arr1.push(arr2[i]);
				// }
				// console.log('arr1',arr1);
				//log打印得到arr1[1,2,3,4];

				//2、concat方法更新数组
				// arr1 = arr1.concat(arr2);

				//3、push.apply方法更新数组
				// arr1.push.apply(arr1, arr2);

				//4、ES6写法合并数组
				// arr1 = [...arr1,...arr2];

				//5、push结合ES6写法合并数组
				// arr1.push(...arr2);


				//删除数组内元素的方法
				// 1.这个函数扩展了JavaScript的内置对象Array，这样，我们以后的所有声明的数组都会自动的拥有remove能力
				// Array.prototype.removeByValue = function (val) {
				//   for (var i = 0; i < this.length; i++) {
				//     if (this[i] === val) {
				//       this.splice(i, 1);
				//       i--;
				//     }
				//   }
				//   return this;
				// }
				// var sites = ['apple', 'google', 'runoob', 'taobao'];
				// sites.removeByValue('google');
				// console.log(sites);
				// -> ['apple', 'runoob', 'taobao']

				//2、array.splice 的方法实现
				// const array = [2, 5, 9, 11];
				// const index = array.indexOf(2);
				// if (index > -1) {
				//   array.splice(index, 1); // 第二个参数为删除的次数，设置只删除一次
				// }
				// console.log('array',array);
				// // array = [2, 9]

				//3、delete关键字,删除数组的值但是它的位置还存在，为empty,arr的长度仍然为4
				// var arr = [1, 2, 3, 4];
				// delete arr[0];
				// console.log(arr);   //[empty, 2, 3, 4]

				//4、栈方法
				var colors = ["A", "B", "C", "D"];
				var item = colors.pop();
				console.log(item); //"C"
				console.log(colors.length); //2

				//5、splice()方法删除
				// var colors = ["red", "blue", "grey"];
				// var item = colors.splice(0, 1);
				// console.log(item);      //"red"
				// console.log(colors);    //["blue", "grey"]
			},

			openPop() {
				console.log('自定义弹窗');
				uni.navigateTo({
					url: '/pages/transmitPage/musicPop',
				})
			},
		}
	}
</script>

<style>
	@import url("pageA.css");
</style>
