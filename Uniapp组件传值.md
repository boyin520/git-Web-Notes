<https://blog.csdn.net/syleapn/article/details/97276991>



一、父组件向子组件传值

通过props来实现，子组件通过props来接收父组件传过来的值！

1、逻辑梳理

父组件中：

第一步:引入子组件；

import sonShow from '../../component/son.vue';
第二步:在components中对子组件进行注册；

components: {
			sonShow
		},
第三步:以标签的形式载入；通过数据绑定的形式进行传值~

<son-show :reciveUserInfo="userInfo"></son-show>

子组件中：

通过props接收父组件中传过来的值；

props:["reciveUserInfo"],


2、代码展示

父组件index.vue

<template>
	<view class="content">
		<son-show :reciveUserInfo="userInfo"></son-show>
	</view>
</template>

<script>
	import sonShow from '../../component/son.vue';
	export default {
		components: {
			sonShow
		},
		data() {
			return {
				userInfo: [{
						"userName": "kaliwo",
						"age": "19"
					},
					{
						"userName": "lihuahua",
						"age": "39"
					}
				]
			}
		}
	}
</script>
子组件son.vue

<template>
	<view class="">
		<block  v-for="(item,index) in reciveUserInfo" :key="index">
			<view class="mesg">
				<text>{{item.userName}}</text>
				<text>{{item.age}}</text>
			</view>
		</block>
	</view>
</template>

<script>
	export default{
		props:["reciveUserInfo"],
	}
</script>
<style>
	.mesg{
		display: flex;
		flex-direction: column;
		align-items: center;
	}
</style>


3、结果



四、说明

对于一些详情页，比如有时我们需要点赞数量+1，-1的效果；但是，由于子组件不能改变父组件的值，所以直接操作从父组件接收的值进行更改是没有效果的！就像如下：

     let list = that.reciveUserInfo;
        for(var i in list){
    	let tempAge = list[i].age + 1;
    	list[i].age = tempAge;
    	that.reciveUserInfo = list;
    }
年龄还是没有改变。所以应该怎么做了？

把从父组件接收到的值reciveUserInfo赋给一个新的变量mesgShow，对这个新的变量进行操作，然后用对齐进行渲染即可！

let list = that.reciveUserInfo;
	for(var i in list){
	   let tempAge = list[i].age + 1;
	   list[i].age = tempAge;
	   that.mesgShow = list;
}
此时的结果为：age+1



附加：改变的代码：



 

二、子组件向父组件传值

与微信小程序自定义组件中子组件向父组件传值一样的逻辑，都是通过事件，下面直接上代码：

父组件index.vue

<template>
	<view class="content">
		<son-show @send="getSonValue"></son-show>
	</view>
</template>

<script>
	import sonShow from '../../component/son.vue';
	export default {
		components: {
			sonShow
		},
		methods:{
			getSonValue: function(res){
				console.log("res=========",res)
			}
		}
	}
</script>
子组件；

<template>
	<view class="" @click="sendMegToIndex">
		点我向父组件传值
	</view>
</template>

<script>
	export default{
		methods:{
			sendMegToIndex: function(){
				// 向父组件传值
				this.$emit("send","我来自子组件")
			}
		}
	}
	
</script>


最终结果：

