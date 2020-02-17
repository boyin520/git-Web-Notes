# Colorui组件库--UniApp版本！



## 前言

[ColorUI是我开发的一款原生小程序组件库](https://www.jianshu.com/p/b81cacf1ccee)，现在移植到了 [UniApp](https://links.jianshu.com/go?to=https%3A%2F%2Funiapp.dcloud.io%2F) 啦,多端开发的前端/设计解决方案哟。
这是我的[Github地址](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fweilanwl%2FColorUI),这是UniApp社区的[插件下载地址](https://links.jianshu.com/go?to=https%3A%2F%2Fext.dcloud.net.cn%2Fplugin%3Fid%3D239)(现在在参加插件比赛，小伙伴觉得不错可以去下载支持下，嘻嘻嘻)

## 开始使用

下载源码解压，复制根目录的 `colorui/` 文件夹到你的根目录

`App.vue` 引入关键Css `main.css` `icon.css`

```
<style>
    @import "colorui/main.css";
    @import "colorui/icon.css";
    @import "app.css"; /* 你的项目css */
    ....
</style>
```

------

## 使用自定义导航栏

`main.js` 引入 `cu-custom` 组件

```
import cuCustom from './colorui/components/cu-custom.vue'
Vue.component('cu-custom',cuCustom)
```

`App.vue` 获得系统信息

```
onLaunch: function() {
    uni.getSystemInfo({
        success: function(e) {
            // #ifndef MP
            Vue.prototype.StatusBar = e.statusBarHeight;
            if (e.platform == 'android') {
                Vue.prototype.CustomBar = e.statusBarHeight + 50;
            } else {
                Vue.prototype.CustomBar = e.statusBarHeight + 45;
            };
            // #endif

            // #ifdef MP
            Vue.prototype.StatusBar = e.statusBarHeight;
            let custom = wx.getMenuButtonBoundingClientRect();
            Vue.prototype.Custom = custom;
            Vue.prototype.CustomBar = custom.bottom + custom.top - e.statusBarHeight;
            // #endif
        }
    })
},
```

`pages.json` 配置取消系统导航栏

```
"globalStyle": {
    "navigationStyle": "custom"
},
```

`page.vue` 页面可以直接调用了

```
<cu-custom bgColor="bg-gradual-blue" :isBack="true">
    <block slot="backText">返回</block>
    <block slot="content">导航栏</block>
</cu-custom>
```

| 参数    | 作用         | 类型    | 默认值 |
| ------- | ------------ | ------- | ------ |
| bgColor | 背景颜色类名 | String  | ''     |
| isBack  | 是否开启返回 | Boolean | false  |
| bgImage | 背景图片路径 | String  | ''     |

| slot块   | 作用                               |
| -------- | ---------------------------------- |
| backText | 返回时的文字                       |
| content  | 中间区域                           |
| right    | 右侧区域(小程序端可使用范围很窄！) |

------

## 使用自定义Tabbar

这部分暂时没有封装，思路可以参考下我的源码，原理是一个主页面引入多个页面，在主页面进行切换显示。这样可以解决切换时闪烁的问题。