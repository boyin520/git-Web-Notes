# uni-app 结合 colorUI 开发项目的整体基本流程

## 一、环境搭建

使用 `HBuilderX`可视化界面快速创建项目，`HBuilderX`内置处理了相关环境依赖。

- HBuilderX：内置uni-app编辑及项目模板，[下载地址](https://www.dcloud.io/hbuilderx.html)
- 微信开发者工具：调试预览工具。[下载地址](https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html)

## 二、创建uni-app项目

### 创建

在`HBuilderX`编辑器中点击工具栏里的`文件 -> 新建 -> 项目`：

![img](https://user-gold-cdn.xitu.io/2019/9/12/16d25e465646801e?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

选择uni-app，输入项目名称，这里有很多模板，选择你所需要的。为了之后表述方便，这里先创建了`ColorUI模板`，为了方便以后引用ColorUI。然后有创建了一个`默认模板`，这个默认模板文件很少，方便以后自由开发。如下两图所示。

![img](https://user-gold-cdn.xitu.io/2019/9/13/16d29c6ee89f5620?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

![img](https://user-gold-cdn.xitu.io/2019/9/13/16d29c57b82d1d83?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 项目目录

| 文件           | 作用                                                         |
| -------------- | ------------------------------------------------------------ |
| pages.json     | 配置页面路由、导航条、选项卡等页面类信息                     |
| manifest.json  | 配置应用名称、appid、logo、版本等打包信息                    |
| App.vue        | 应用配置，用来配置App全局样式以及监听应用的生命周期          |
| main.js        | Vue初始化入口文件                                            |
| static目录     | 存放应用引用静态资源（如图片、视频等）的地方，注意：静态资源只能存放于此 |
| pages目录      | 业务页面文件存放目录                                         |
| components目录 | 组件文件存放目录                                             |

注意：在开发过程中，当你写登录功能后，某个页面你需要经常进入并且多次修改，你每次修改都需要登录后的信息，所以就很麻烦了。你需要一直从登录开始操作，直到你到达这个界面。这时候你就可以用这个了`condition`。

**现在来介绍一下condition：**

**condition** 启动模式配置，仅开发期间生效，用于模拟直达页面的场景，如：小程序转发后，用户点击所打开的页面。

**属性说明：**

| 属性    | 类型   | 是否必填 | 描述                             |
| ------- | ------ | -------- | -------------------------------- |
| current | Number | 是       | 当前激活的模式，list节点的索引值 |
| list    | Array  | 是       | 启动模式列表                     |

**list说明：**

| 属性  | 类型   | 是否必填 | 描述                                   |
| ----- | ------ | -------- | -------------------------------------- |
| name  | String | 是       | 启动模式名称                           |
| path  | String | 是       | 启动页面路径                           |
| query | String | 否       | 启动参数，可在页面的 onLoad 函数里获得 |

在 5+App 里真机运行可直接打开配置的页面，微信开发者工具里需要手动改变编译模式，过程：微信开发工具 -> 工具 -> 编译配置 -> 找到在`condition`里配置的`name`。

在`pages.josn`里配置`condition`，代码如下：

```
"condition": { //模式配置，仅开发期间生效
    "current": 0, //当前激活的模式（list 的索引项）
    "list": [{
             "name": "jingdian_detail", //模式名称
	     "path": "pages/jingdian_detail/jingdian_detail", //启动页面，必选
	     "query": "id=5d1483336d724301607b2c23" //启动参数，在页面的onLoad函数里面得到。
	   }
    ]
}
复制代码
```

![img](https://user-gold-cdn.xitu.io/2019/9/23/16d5ec340f91df47?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

## 三、引用ColorUI

有了第二步创建好的模板，我们就可以引用了。

### ColorUI的引入

首先复制一下`ColorUI`项目目录下的`colorui`目录，然后粘贴到`demo`项目的根目录下，与`pages`目录同级。

![img](https://user-gold-cdn.xitu.io/2019/9/13/16d29f99068ca72e?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 使用顶部操作条

如果喜欢ColorUI里自定义的`navigationBar`，即`cu-custom`组件，那就需要在`demo`项目中的`main.js`里导入。建议还是使用`cu-custom`组件。

```
/* demo/main.js */
import cuCustom from './colorui/components/cu-custom.vue'
Vue.component('cu-custom',cuCustom)
复制代码
```

下图就是ColorUI的状态栏：

![img](https://user-gold-cdn.xitu.io/2019/9/13/16d2a066bebb579d?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

你可以直接复制`ColorUI`项目里的`App.vue`文件，因为这里面设置好了`cu-custom`组件的高度以及定义了全局的颜色。并且还需要将`pages.json`配置取消系统导航栏。（开启 custom 后，所有窗口均无导航栏）

```
"globalStyle": {
	"navigationStyle": "custom"
},
复制代码
```

如果不想用`cu-custom`组件，那就直接在`demo`项目里的`App.vue`文件的`<style></style>`里引入这两句话就OK了。当然上面也需要加。

```
@import "colorui/main.css";
@import "colorui/icon.css";
复制代码
```

关于引用ColorUI的介绍，也可以查看[引用ColorUI](https://github.com/weilanwl/ColorUI/tree/ae85fc5b01f264c54292e517ff574e1bb1c47d5c/Colorui-UniApp)

### 底部操作条。

![img](https://user-gold-cdn.xitu.io/2019/9/13/16d2a0a43127c3a7?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

```
/*  ColorUI/pages/index/index.vue  */
<template>
	<view>
	
	    //使用v-if条件判断，来显示相应的界面 
	    //如果点击了底部中的“操作条元素->basics”、“组件->component” 或者 “扩展->plugin”，界面会出现相应的主界面。如上图所示。
		<basics v-if="PageCur=='basics'"></basics>
		<components v-if="PageCur=='component'"></components>
		<plugin v-if="PageCur=='plugin'"></plugin>

            //这里的“元素”，“组件”，“扩展”三个操作都是绑定一个点击事件NavChange，通过设置PageCur的值再结合:class属性来改变图片的路径，进而改变了按钮的颜色
		<view class="cu-bar tabbar bg-white shadow foot">
			<view class="action" @click="NavChange" data-cur="basics">
				<view class='cuIcon-cu-image'>
					<image :src="'/static/tabbar/basics' + [PageCur=='basics'?'_cur':''] + '.png'"></image>
				</view>
				<view :class="PageCur=='basics'?'text-green':'text-gray'">元素</view>
			</view>
			<view class="action" @click="NavChange" data-cur="component">
				<view class='cuIcon-cu-image'>
					<image :src="'/static/tabbar/component' + [PageCur == 'component'?'_cur':''] + '.png'"></image>
				</view>
				<view :class="PageCur=='component'?'text-green':'text-gray'">组件</view>
			</view>
			<view class="action" @click="NavChange" data-cur="plugin">
				<view class='cuIcon-cu-image'>
					<image :src="'/static/tabbar/plugin' + [PageCur == 'plugin'?'_cur':''] + '.png'"></image>
				</view>
				<view :class="PageCur=='plugin'?'text-green':'text-gray'">扩展</view>
			</view>
		</view>
	</view>
</template>
<script>
	export default {
		data() {
		return {
				PageCur: 'basics'
			}
		},
		methods: {
			NavChange: function(e) {
				this.PageCur = e.currentTarget.dataset.cur
			}
		}
	}
</script>
<style>
</style>

复制代码
```

当点击“元素”，“组件”，“扩展”三个按钮的其中之一时，都会出现相应的页面，原因就是在ColorUI的项目里`mian.js`里要引入下面的代码：

```
import basics from './pages/basics/home.vue'
Vue.component('basics',basics)

import components from './pages/component/home.vue'
Vue.component('components',components)

import plugin from './pages/plugin/home.vue'
Vue.component('plugin',plugin)

复制代码
```

这个底部操作条还有其它方式，可以查看这个[底部操作条](https://github.com/weilanwl/ColorUI/blob/ae85fc5b01f264c54292e517ff574e1bb1c47d5c/Colorui-UniApp/pages/component/bar.vue)

## 四、结合后端基本操作的实现

### 实现用户登陆注册

#### 简单注册

上前端代码

```
<view class="list">
	<view class="list-call">
		<text class="cuIcon-mobile text-style "></text>
		<input class="biaoti" v-model="phoneNum" type="number" maxlength="11" placeholder="输入手机号" />
	</view>
	<view class="list-call">
		<text class="cuIcon-lock text-style "></text>
		<input class="biaoti" v-model="password" type="text" 
		       maxlength="32" placeholder="输入密码" :password='tfpsw' />
		<image class="img" 
		       :src="showPassword?'../../static/login-reg-forget/op.png':'../../static/login-reg-forget/cl.png'"
		       @tap="display"></image>
	</view>
	<view class="list-call">
		<text class="cuIcon-people text-style "></text>
		<input class="biaoti" v-model="username" type="text" placeholder="输入用户名" />
	</view>
</view>
	
<view class="dlbutton bg-color" hover-class="dlbutton-hover" @tap="bindReg">
	<text>注册</text>
</view>
<script>

export default {
	data() {
		return {
			phoneNum:'',
			password:'',
			username:'',
			tfpsw:true,
			xieyi:true,
			showPassword:false,
		};
	},
	methods: {
		//密码显示或打马	
		display() {
		    this.showPassword = !this.showPassword
			this.tfpsw =!this.tfpsw
		},
		//协议
		xieyitong(){
			this.xieyi = !this.xieyi;
		},
		//注册
	        bindReg() {
	        //简单验证
			if (this.xieyi == false) {
			    uni.showToast({
			        icon: 'none',
			        title: '请先阅读《软件用户协议》'
			    });
			    return;
			}
			if (this.phoneNum.length !=11) {
			    uni.showToast({
			        icon: 'none',
			        title: '手机号不正确'
			    });
			    return;
			}
			if (this.password.length < 6) {
			    uni.showToast({
			        icon: 'none',
			        title: '密码位数至少6位'
			        
			    });
			    return;
			}
			//将信息传到后台
			uni.request({
			//this.websiteUrl是全局api，在mian.js里设置
			    url: this.websiteUrl+'/user/signup',
			    data: {
			        mobile:this.phoneNum,
			        password:this.password,
			        username:this.username,
			    },
			    method: 'POST',
			    dataType:'json',
			    success: (res) => {
    			    if(res.statusCode==200){
    			        uni.reLaunch({
    			            url: "../login/login" //跳转到首页
    			    });
			        uni.showToast({title:"注册成功！",icon:'none'});
			    }else{
			        uni.showToast({title:"注册失败",icon:'wrong'});
			    }
			}
		});
	 }
    }
}
</script>
//样式表就不加啦
复制代码
```

uni.request 是发起网络请求 [查看更多](https://user-gold-cdn.xitu.io/2019/9/15/16d356e1a8ca4996)，在这里我就提一下`url: this.websiteUrl+'/user/signup',`这个`url`里的`this.websiteUrl`,这个是在`main.js`里定义的全局api。

```
//设置全局的api地址
Vue.prototype.websiteUrl = 'http://10.4.14.132:7000';
复制代码
```

其中`10.4.14.132`是本地的`IPV4`，`:7000`是后端的端口号。

结合运行界面，更加直观（图有点大......）

![img](https://user-gold-cdn.xitu.io/2019/9/15/16d355970c97f209?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

#### 登陆

这次换了一个手机类型，现在小了很多，先看看界面吧！

![img](https://user-gold-cdn.xitu.io/2019/9/15/16d3585aa255fbce?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

```
<view class="list">
    <view class="list-call">
        <text class="cuIcon-mobile text-style "></text>
	<input class="biaoti" v-model="phoneNum" type="number" maxlength="11" placeholder="输入手机号" />
    </view>
    <view class="list-call">
	<text class="cuIcon-lock text-style "></text>
	<input class="biaoti" v-model="password" type="text" maxlength="32" placeholder="输入密码" password="true" />
    </view>
</view>
<view class="dlbutton bg-color" hover-class="dlbutton-hover" @tap="bindLogin()">
    <text>登录</text>
</view>

<script>
export default {
    data() {
        return {
		phoneNum: '',
		password: ''
	    };
	},
	
	methods: {
	    bindLogin() {
		if (this.phoneNum.length != 11) {
		    uni.showToast({
			icon: 'none',
			title: '手机号不正确'
		    });
		    return;
		}
		if (this.password.length < 6) {
			uni.showToast({
			    icon: 'none',
			    title: '密码不正确'
			});
			return;
		}
		uni.request({
			url: this.websiteUrl+'/user/login',
			data: {
				mobile: this.phoneNum,
				password: this.password
			},
			method: 'POST',
			dataType: 'json',
			success: (res) => {
			    console.log("登录返回数据",res);
			    if (res.data.data.code == 1) {
				uni.showToast({
			            title: "登录成功！",
			    	    icon: 'none'
				});
			        uni.setStorageSync('token', res.data.data.token);
			        uni.setStorageSync('loginUser',res.data.data.userinfo);
			        uni.reLaunch({
			            url: "../index/index" //跳转到首页
			        });
			    } else {
				uni.showToast({
				    title: "登录失败！",
				    icon: 'wrong'
				});
			 }
		    }
	       });
        }
    }
}
</script>
复制代码
```

在登陆的时候用到了`uni.setStorageSync`，`uni.setStorageSync`将 data 存储在本地缓存中指定的 key 中，会覆盖掉原来该 key 对应的内容，这是一个同步接口。当使用`uni.setStorageSync`接口将数据存到本地缓存中时，就需要`uni.getStorageSync`来获取信息。[查看更多](https://uniapp.dcloud.io/api/storage/storage?id=setstoragesync)

![img](https://user-gold-cdn.xitu.io/2019/9/15/16d3594dfda6c9d6?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### class 与 style 的绑定

在开发中会使用到`class` 与 `style` 的绑定，来介绍一下：

class 支持的语法:

```
<view :class="{ active: isActive }">111</view>
<view class="static" v-bind:class="{ active: isActive, 'text-danger': hasError }">222</view>
<view class="static" :class="[activeClass, errorClass]">333</view>
<view class="static" v-bind:class="[isActive ? activeClass : '', errorClass]">444</view>
<view class="static" v-bind:class="[{ active: isActive }, errorClass]">555</view>
复制代码
```

style 支持的语法:

```
<view v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }">666</view>
<view v-bind:style="[{ color: activeColor, fontSize: fontSize + 'px' }]">777</view>
复制代码
```

接下来我们来写个例子：

当我们提交“申请”之前，按钮的颜色是绿色的，符合用户的习惯。当点击按钮之后，会进行一系列的操作，比如与后端进行交互，这个例子比较简单，就是普通的改变样式。

html:

```
<button class="static" 
        :class="[isSubmit ? Submited: '', NoSubmit]"
        @click="submitFile">{{text}}</button>
复制代码
```

js: 一定要在data里注册后，再在class里使用，这样才会被渲染出来。

```
export default {
	data() {
		return {
			Submited: 'Submited',
			NoSubmit: 'NoSubmit',
			isSubmit:false,
			text:'申请'
		}
	},
	methods: {
	    submitFile(){
			this.text ='已申请';
			this.isSubmit = true;
		}
	}
}
复制代码
```

### 页面的跳转

这里就简单说一下路由跳转问题，详情可以看[官网](https://uniapp.dcloud.io/api/router?id=navigateto)。

#### uni.navigateTo() : 跳转非tarBar页面

保留当前页面，跳转到应用内的某个页面，使用`uni.navigateBack`可以返回到原页面。

示例：

```
//在起始页面跳转到test.vue页面并传递参数
uni.navigateTo({
    url: 'test?id=1&name=uniapp'
});
复制代码
```

```
// 在test.vue页面接受参数
export default {
    onLoad: function (option) { //option为object类型，会序列化上个页面传递的参数
        console.log(option.id); //打印出上个页面传递的参数。
        console.log(option.name); //打印出上个页面传递的参数。
    }
}
复制代码
```

#### uni.redirectTo() : 跳转非tarBar页面

关闭当前页面，跳转到应用内的某个页面。 示例:

```
uni.redirectTo({
    url: 'test?id=1'
});
复制代码
```

#### uni.reLaunch()

关闭所有页面，打开到应用内的某个页面。

需要跳转的应用内页面路径 , 路径后可以带参数。参数与路径之间使用`?`分隔，参数键与参数值用`=`相连，不同参数用`&`分隔；如 `path?key=value&key2=value2`，如果跳转的页面路径是 tabBar 页面则不能带参。

#### uni.switchTab(OBJECT)

跳转到 tabBar 页面，并关闭其他所有非 tabBar 页面。 示例：

pages.json

```
{
  "tabBar": {
    "list": [{
      "pagePath": "pages/index/index",
      "text": "首页"
    },{
      "pagePath": "pages/other/other",
      "text": "其他"
    }]
  }
}

复制代码
```

other.vue

```
uni.switchTab({
    url: '/pages/index/index'
});
复制代码
```

#### 小总结：

- navigateTo, redirectTo 只能打开非 tabBar 页面。
- switchTab 只能打开 tabBar 页面。
- reLaunch 可以打开任意页面。
- 页面底部的 tabBar 由页面决定，即只要是定义为 tabBar 的页面，底部都有 tabBar。
- 不能在 App.vue 里面进行页面跳转。

### 页面传参

就比如点击这个景点的卡片，会进入这个景点的详情页面，这个过程中就用到了页面的传参。

![img](https://user-gold-cdn.xitu.io/2019/9/25/16d68fb36a65cdbe?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

主要代码如下：

```
//index.vue
<template>
<view class="cu-card case "  v-for="(item, index) in jingdian" :key="index"  @click="jd_Detial(item)">
	<view class="cu-item shadow">
		<view class="image">
			<image :src="item.img" mode="widthFix"></image>
			<view class="cu-tag bg-red">Hot</view>
		</view>
		<view class="cu-list menu-avatar">
			<view class="cu-item">
				<view class="content flex-sub" style="left: 20px;">
					<view class="text-grey">{{item.name}}</view>
					<view class="text-gray text-sm flex justify-between">
						<view class="text-gray text-sm">
							<text class="icon-icon_like-copy margin-lr-xs"></text>收藏
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</view>
</template>

<script>
	export default {
		data() {
			return {
			    jingdian:[]
			}
		},
		created:function () {
			try {
			    //获取所有景点的数据
				uni.request({
					url: this.websiteUrl+'/api/showCityTickets/'+ this.cityId,
					method: 'GET',
					dataType: 'json',
					success: (res) => {
						this.jingdian = res.data.data;
					}
				})
			} catch (e) {
				// error
				console.log('error',e)
			}
		},
		methods: {
		    jd_Detial(item){
				console.log('id',item._id);
				//页面跳转以及传到下一页面所需要的参数_id，
				uni.navigateTo({
					url:"../jingdian_detail/jingdian_detail?id=" + item._id
				})
			}
		}
	}
</script>			
复制代码
```

下面是`jingdian_detail.vue`的详情页面

![img](https://user-gold-cdn.xitu.io/2019/9/25/16d69061f7884726?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

```
jingdian_detail.vue
```

```
onLoad()
```

```
_id
```

```
onLoad(options) {
	var _this = this;
	this.ticketId = options.id;
	//根据这个ID来获取相应的景点的详情数据，根据详情数据再来渲染出详情页面
	uni.request({
		url: this.websiteUrl + '/api/ticketsShowDetial/' + options.id,
		method: 'GET',
		dataType: 'json',
		success: function success(res) { 
			if (res.statusCode == 200) {
				_this.detail = res.data.data;
			}
		}
	});
	console.log('&&&&&&&&this.detail&&&&&&', this.detail);
}
复制代码
```

### 获取位置

自己写了一个简单的定位，只是为了方便大家了解这一功能是如何实现的。

这个获取位置只能在手机端进行测试，H5端没有GPS。

上代码：

```
<template>
<view style="margin: 10px auto;">
	<button @click="getLocal()">定位</button>
	<p>经度:{{longitude}} </p>
	<p>纬度：{{latitude}}</p>
	<p>详细地址{{address.country+address.province+address.city + address.district+ address.street}}</p>
</view>
</template>
<script>
	export default {
	    data() {
		return {
		    latitude:'',//纬度
		    longitude:'',//经度
		    address:{},//地址信息			
		 }
	    },
        methods: {
        	getLocal(){
        		// 获取当前的地理位置、速度。 
        		let that = this;
        		try{
        			uni.getLocation({
        			    type: 'wgs84',
        			    geocode:true,//解析地址信息
        			    success: function (res) {
        					that.latitude =  res.latitude
        					that.longitude = res.longitude
        					that.address = res.address
        					//查看地图
        					 uni.openLocation({
        					            latitude: that.latitude,
        					            longitude: that.longitude,
        					            success: function () {
        					                console.log('打开地图success');
        					            }
        					 });
        			    },
        			    fail:function(res){
        				    console.log("fail====",res)
        			    }
        			});
        		}catch(e){
        			console.log("error",e)
        		}
            }
        }
    }
复制代码
```

点击定位按钮，获取本地的经纬度和详细地址，然后会自动跳到地图页面。

![img](https://user-gold-cdn.xitu.io/2019/9/27/16d71b9c2fd2c29c?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

![img](https://user-gold-cdn.xitu.io/2019/9/27/16d71b6823487255?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

获取位置

查看地图

## 运行和发布