# uni-app 利用自带swiper实现点击tab内容滑动，滑动内容tab切换（不使用插件）

结构

```
<view class="ui-car-select">
	<view class="ui-car-name-list">
		<view class="ui-car-name-item" v-for="(item,index) in carTypes" :class="{active:current==index}" :data-current="index" @tap="tabChange">{{item.cartype}}</view>
	</view>
	<view>
		<swiper class="ui-carinfos" @change="swiperChange" :current="current">
			<swiper-item class="ui-carinfo-item" v-for="(item,index) in carinfos">{{item.carinfo}}</swiper-item>
		</swiper>
	</view>
</view>
```


代码

```
data() {
			return {
				visible: false,
				current:0,
				carTypes:[{
					cartype:"小面包"
					},{
						cartype:"中面包"
					},{
						cartype:"小货车"
					},{
						cartype:"中货车"
				}],
				carinfos:[{
					carinfo:"小面包"
					},{
						carinfo:"中面包"
					},{
						carinfo:"小货车"
					},{
						carinfo:"中货车"
				}]
			}
		},
methods:{
	tabChange:function(e){
		var index = e.target.dataset.current || e.currentTarget.dataset.current;
		this.current=index;
	},
	swiperChange:function(e) {			
		var index=e.target.current || e.detail.current;
		this.current = index;
	}
}
```


记得给tab添加.active类名的样式。这里就不写了。

给tab的每一个加data-current，利用e.target.dataset.current赋值index。swiper利用自带current属性，利用e.target.current赋值index。

不知道会不会有兼容问题，或者其他的注意点。有知道的朋友欢迎留言。我也是第一次用，开始没看到这个费了老大劲了。

可以参考hello uniapp template模板里面的tabbar>tabbar.nvue。
————————————————
