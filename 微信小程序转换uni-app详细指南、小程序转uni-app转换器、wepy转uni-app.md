# 微信小程序转换uni-app详细指南、小程序转uni-app转换器、wepy转uni-app

分类：uni-app

[小程序转为uni_app](https://ask.dcloud.net.cn/topic/%E5%B0%8F%E7%A8%8B%E5%BA%8F%E8%BD%AC%E4%B8%BAuni_app) [小程序转成uni_app](https://ask.dcloud.net.cn/topic/%E5%B0%8F%E7%A8%8B%E5%BA%8F%E8%BD%AC%E6%88%90uni_app) [小程序转uni_app](https://ask.dcloud.net.cn/topic/%E5%B0%8F%E7%A8%8B%E5%BA%8F%E8%BD%ACuni_app) [小程序转换](https://ask.dcloud.net.cn/topic/%E5%B0%8F%E7%A8%8B%E5%BA%8F%E8%BD%AC%E6%8D%A2)

本文首先分享转换步骤和原理，文末会分享三方开发者制作的**小程序转uni-app的转换器**和**wepy转uni-app转换器**。

小程序转换uni-app的步骤

微信小程序的语法，其实是vue.js语法的裁剪定制版，在数据绑定、自定义组件等很多方面都有相似之处。
以下是一个小程序源码转换步骤指南：

## 客户端代码转换

1. 新建一个uni-app项目，把之前的app.js、app.wxss的代码，挪到app.vue里，分别放到script和style节点下面

   ​

   如果其中有globalData等全局变量或方法，也直接放到app.vue的script下

   ```
   复制代码export default {    
       globalData: {    
           text: 'text'    
       },    
       onLaunch: function() {    
           console.log('App Launch')    
       }  
   }  
   ```

   读取globalData或赋值的方法是`getApp().globalData.text = 'test'`

2. 转换app.json为pages.json，把每个小程序page目录下的index.json（或页面名称对应的json）里的配置取出来，放到pages.json的style下

3. pages下每个页面目录放一个vue空文件模板

4. 把之前页面的wxss内容复制到vue文件的style中，无需改动

5. 把之前页面的js内容复制到vue文件的script中，然后执行如下改动

   - 5.1 之前js里面的data，放到新的data return下

     ​

     之前

     ```
     复制代码Page({  
     data: {  
     show1: false  
     }  
     })
     ```

     现在

     ```
     复制代码<script>  
     export default {  
     data() {  
         return {  
             show1: false  
         }  
     }  
     }  
     </script>
     ```

   - 5.2 之前js里面的自定义方法，放到新的method下

     ​

     之前

     ```
     复制代码Page({  
     toggleActionSheet1() {  
     this.setData({show1: true});  
     }  
     })
     ```

     现在

     ```
     复制代码<script>  
     export default {  
     methods: {  
         toggleActionSheet1() {  
             this.show1 = true  
         }  
     }  
     }  
     </script>
     ```

   - 5.3 之前js里面的生命周期函数onLoad、onShow等，直接放到export default下

     ​

     之前

     ```
     复制代码Page({  
     onLoad() {  
     console.log("page load");  
     }  
     })
     ```

     现在

     ```
     复制代码<script>  
     export default {  
     onLoad() {  
         console.log("page load");  
     }  
     }  
     </script>
     ```

   - 5.4 setdata的处理方式

   - 方式一：从 `this.setData({loading: false,areaList: response.data.data})` 改为 `this.loading = false;this.areaList = response.data.data`。

   - 方式二：重写setdata方法，如下

     ```
     复制代码setData:function(obj){    
     let that = this;    
     let keys = [];    
     let val,data;    
     Object.keys(obj).forEach(function(key){    
      keys = key.split('.');    
      val = obj[key];    
      data = that.$data;    
      keys.forEach(function(key2,index){    
          if(index+1 == keys.length){    
              that.$set(data,key2,val);    
          }else{    
              if(!data[key2]){    
                 that.$set(data,key2,{});    
              }    
          }    
          data = data[key2];    
      })    
     });    
     }  
     ```

     *以上代码出处：https://ask.dcloud.net.cn/article/35020*

6. 把之前页面的wxml内容复制到vue文件的template下的view下，然后执行如下改动

   - 属性绑定从
     attr="{{ a }}"，改为 :attr="a"
     title="复选框{{ item }}" 改为 :title="'复选框' + item"
   - 事件绑定从 bind:click="toggleActionSheet1" 改为 @click="toggleActionSheet1"
   - 阻止事件冒泡 从 catch:tap="xx" 改为 @tap.native.stop="xx"
   - `wx:if` 改为 `v-if`
   - wx:for="{{ list }}" wx:key="{{ index }}" 改为`v-for="(item,index) in list"

7. 微信小程序自定义组件处理

   ​

   之前引入的自定义组件，需要放到wxcomponents下，并在pages.json里注册。如果这里有js，并且被其他代码引入，要注意修改引用代码的路径指向。如下：

   ```
   复制代码{  
   "pages": [ //pages数组中第一项表示应用启动页，参考：https://uniapp.dcloud.io/collocation/pages  
       {"path": "pages/dashboard/dashboard"},  
       {  
           "path": "pages/action-sheet/action-sheet",  
           "style":{  
               "navigationBarTitleText":"ActionSheet 上拉菜单",  
               "usingComponents":{ //这里单页面引入action-sheet组件  
                   "van-action-sheet": "/wxcomponents/vant/action-sheet/index"  
               }  
           }  
       }  
   ],  
   "globalStyle": {  
       "navigationBarTitleText": "Vant For Uni-app",  
       "usingComponents": { //这里给所有页面全局引入了如下组件  
           "demo-block": "/wxcomponents/vant/demo-block/index",  
           "van-button": "/wxcomponents/vant/button/index",  
           "van-cell": "/wxcomponents/vant/cell/index",  
           "van-cell-group": "/wxcomponents/vant/cell-group/index",  
           "van-icon": "/wxcomponents/vant/icon/index",  
           "van-loading": "/wxcomponents/vant/loading/index",  
           "van-toast": "/wxcomponents/vant/toast/index"  
       }  
   }  
   }
   ```

   微信自定义组件虽然可以这样转换。但转换后只能用于微信和App。如果想用于支付宝百度头条，则需要新建swancomponents等目录，将微信自定义组件复制到这些目录，改造测试。虽然各小程序平台均支持自定义组件，但细节有差异，仍需自己测试。无论如何，H5端不支持这些自定义组件。
   比较妥善的跨端做法，是在uni-app插件市场寻找类似功能的vue组件，废弃之前的小程序自定义组件。比如把wx-charts换成ucharts、把wx-parser换成uparser。

### 辅助工具

贡献几个替换用的正则

```
复制代码str = str.replace(/bindtap/g, '@onclick');  
str = str.replace(/wx:if/g, 'v-show');  
str = str.replace(/src=\'\{\{/g, ":src='");  
str = str.replace(/wx\:key=\"\*this\"/g, ' ');  
str = str.replace(/wx\:key\=\"index\"/g, ' ');  
str = str.replace(/wx:for="{{/g, v-for= "(item,index) in ');  
str = str.replace(/bindinput/g, '@input'); 
```

### `wx.`是否要替换为`uni.`？

关于js api中的`wx.`，不要全局替换为`uni.`。因为有的wx的api是微信独有的，替换为uni后，反而在微信下没法用了。

同时uni-app编译器提供了把`wx.`编译为不同平台的机制，所以直接使用`wx.`的api完全可以正常在各端运行。

所以对于老代码，替不替换不重要，不影响运行，只影响语法提示和转到定义。

但是新写的代码，还是要用`uni.`的api，在代码提示、转到定义方面更强大。

## App端迁移，还需处理服务端相关代码

如果把微信小程序转换为uni-app，仍然用于发布微信小程序，那服务器端代码不变。

但如果要发布到App、其他小程序等平台，服务器也需要调整部分代码。比如登陆、支付、推送、定位、地图等联网服务。

uni-app在客户端侧提供了统一的代码，比如uni.login、uni.requestPayment，在不同端均可以实现登陆、支付。

但服务器端的接口不一样，比如微信的App支付和小程序支付的申请开通、服务器接口都不一样，所以配置和服务器接口仍需单独处理。

比如把小程序转换uni-app后，需要打包发布为app，则需要向微信申请app支付的资质，拿到appkey等信息，填写到uni-app工程的 manifest-app -> sdk配置 -> 微信支付 下面，然后打包才能成效（如果是离线打包，参考离线打包的文件）。同时服务器需要按照微信的App支付的接口再开发对接。

同样微信小程序里内置的定位、地图，在app上，需要单独向高德等三方服务商申请，否则无法在app里使用这些服务。

这些sdk申请方式在 manifest -> app sdk配置 下有教程链接。

## 其他注意

参考：这里有一个转换示例，把vant weapp的小程序演示demo，转换为uni-app工程，里面的pages下同时保留了wxml和vue，可用于对比参考。<http://ext.dcloud.net.cn/plugin?id=302>

如果是mpvue的小程序，转换uni-app很方面，参考文档：<https://ask.dcloud.net.cn/article/34945>

## 感谢zhangdaren开源了小程序到uni-app的转换器

- 普通wxml小程序转uni-app：<https://github.com/zhangdaren/miniprogram-to-uniapp>
  对应的教程：<https://ask.dcloud.net.cn/article/36037>
  这个转换工具的案例：<https://ext.dcloud.net.cn/plugin?id=666>
- wepy转uni-app转换器：<https://github.com/zhangdaren/wepy-to-uniapp>