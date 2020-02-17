# uni-app导航栏开发指南

**本文虽长，但值得看完。可避免开发中的很多坑**

uni-app 自带原生导航栏，在pages.json里配置。
原生导航的体验更好，渲染新页面时，原生导航栏的渲染无需等待新页面dom加载，可以在新页面进入动画开始时就渲染。

原生导航还可以避免滚动条通顶，并方便的控制原生下拉刷新。
通过pages.json的配置，可以简单的、跨端的、高性能的开发业务。

但原生导航栏的扩展能力有限的。尤其是微信下，没有提供太多导航栏的配置。
在App下，pages.json里每个页面的app-plus下可以设置titleNView等更多参数，可以得到比微信小程序更丰富的扩展性。
另外，开发者也可以在必要时取消原生导航栏，使用view自行绘制导航栏。

## 原生导航栏的通用配置

原生导航栏的配置，均在pages.json里，每个page下面的style配置中的navigationBar各个参数配置，即为通用配置，小程序、app、h5均生效。参考<https://uniapp.dcloud.io/collocation/pages?id=style>

## 全局取消原生导航栏

在pages.json的globalStyle里，有个navigationStyle设置，默认是default，即带有原生导航栏。
也可以设置为custom。
在设为custom后，所有页面都没有原生导航。
但在微信小程序里，右上角始终都有一个胶囊按钮。
很多微信小游戏界面上也没原生导航栏，但有胶囊按钮。
一般App里不会使用这个参数配置。建议个别页面单独设置不使用原生导航，具体见下。

## 单独去除原生导航栏

自微信客户端 7.0.0 起、App端HBuilderX 2.0.3起，支持通过如下方法取消单独一个页面的原生导航栏。但小程序右上角胶囊按钮仍然去不掉。页面配置 navigationStyle 为 custom：

```
复制代码{  
    "path" : "pages/log/log",  
    "style" : {  
        "navigationStyle":"custom"  
    }  
}
```

## 原生导航栏在App侧的扩展

微信小程序的设计里，没有给原生导航提供太多自定义能力。在开发App时是不够用的。
pages.json里，每个page下面的style下面还有一个子扩展节点：app-plus。
这个节点定义了在5+App环境下，也即iOS、Android环境下，增强的配置。
其中有一个子节点titleNView，这个是5+规范里webview页面的原生导航窗体规范。
参考<https://uniapp.dcloud.io/collocation/pages?id=app-plus>

### App单独去除原生导航栏

```
复制代码{  
    "path": "pages/log/log",  
    "style": {  
        "navigationBarTitleText": "hello",  
        "app-plus": {  
            "titleNView": false  
        }  
    }  
}
```

在App去除原生导航栏后，小程序端侧仍保有原生导航栏。

### App去除导航栏后改变状态栏样式

App因为默认为沉浸式，去除导航栏后，页面顶部会直通到状态栏的区域，可能出现如下需求：

- 改变状态栏文字颜色：设置该页面的 navigationBarTextStyle 属性，可取值为 black/white。如果想单独设置颜色，App端可使用[plus.navigator.setStatusBarStyle](http://www.html5plus.org/doc/zh_cn/navigator.html#plus.navigator.setStatusBarStyle)设置。部分低端Android手机（4.4）自身不支持设置状态栏前景色。
- 改变状态栏背景颜色：通过绘制一个占位的view固定放在状态栏位置，设置此view的背景颜色，即可达到想要的效果，uni-app提供了一个状态栏高度的css变量，具体参考：<http://uniapp.dcloud.io/frame?id=css%E5%8F%98%E9%87%8F>

以下为示例：

```
复制代码<!-- #ifdef APP-PLUS -->  
<view class="status_bar">  
    <view class="top_view"></view>  
</view>  
<!-- #endif -->
```

```
复制代码.status_bar {  
    height: var(--status-bar-height);  
    width: 100%;  
    background-color: #F8F8F8;  
}  
.top_view {  
    height: var(--status-bar-height);  
    width: 100%;  
    position: fixed;  
    background-color: #F8F8F8;  
    top: 0;  
    z-index: 999;  
}
```

### 给原生导航栏添加自定义按钮

注意：按钮的点击事件需要在页面监听[onNavigationBarButtonTap](http://uniapp.dcloud.io/use?id=%E9%A1%B5%E9%9D%A2%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)事件

页面监听代码如下：

```
复制代码export default {  
    data() {  
        return {}  
    },  
    onNavigationBarButtonTap() {  
        console.log("点击了自定义按钮");  
    }  
}  
```

pages.json配置如下：

```
复制代码{  
    "path": "pages/log/log",  
    "style": {  
        "navigationBarTitleText": "hello",  
        "app-plus": {  
            "titleNView": {  
                "buttons": [{  
                    "text": "\ue534",  
                    "fontSrc": "/static/uni.ttf",  
                    "fontSize": "22px"  
                }]  
            }  
        }  
    }  
}
```

buttons的text推荐使用字体图标。
**如果按钮使用的汉字或英文较长，推荐把字体改小一点，或者调节按钮宽度等值。**
配置button的背景颜色为透明：background:'rgba(0,0,0,0)'

以上代码在hello uni-app的模板-顶部导航标题栏中有示例。

### 原生导航栏自定义按钮带红点和角标

```
复制代码{  
    "path" : "nav-dot/nav-dot",  
    "style" : {  
        "navigationBarTitleText" : "导航栏带红点和角标",  
        "app-plus" : {  
            "titleNView" : {  
                "buttons" : [  
                    {  
                        "text" : "消息",  
                        "fontSize" : "14",  
                        "redDot" : true  
                    },  
                    {  
                        "text" : "关注",  
                        "fontSize" : "14",  
                        "badgeText" : "12"  
                    }  
                ]  
            }  
        }  
    }  
}
```

以上代码在hello uni-app的模板-顶部导航标题栏中有示例。

### 原生导航栏自定义按钮带下拉选择（城市选择）

```
复制代码{  
    "path" : "nav-city-dropdown/nav-city-dropdown",  
    "style" : {  
        "navigationBarTitleText" : "导航栏带城市选择",  
        "app-plus" : {  
            "titleNView" : {  
                "buttons" : [  
                    {  
                        "text" : "北京市",  
                        "fontSize" : "14",  
                        "select" : true,  
                        "width" : "auto"  
                    }  
                ]  
            }  
        }  
    }  
}
```

以上代码在hello uni-app的模板-顶部导航标题栏中有示例。

### 导航栏上的原生搜索框

原生导航栏支持放置原生搜索框，可点击直接弹出软键盘，也可以点击后跳转到新页面搜索。
因代码较多，此处不列，请参考hello uni-app的模板-顶部导航标题栏示例。
如需动态修改searchInput，或者获取searchInput的placehold，参考[uni-app动态修改App端导航栏](https://ask.dcloud.net.cn/article/35374)

### 配置原生导航栏的透明渐变

原生导航栏还支持透明渐变效果，页面刚载入时没有导航标题，页面内容通顶到状态栏里，页面向下滚动后标题栏渐变出现。

```
复制代码{  
    "path": "pages/log/log",  
    "style": {  
        "navigationBarTitleText": "hello",  
        "app-plus": {  
            "titleNView": {  
                "type": "transparent"  
            }  
        }  
    }  
}
```

实际上可用的titleNView设置还有很多，详细的api见<http://www.html5plus.org/doc/zh_cn/webview.html#plus.webview.WebviewTitleNViewStyles>

透明渐变的导航栏的button图标有一个默认的灰色背景圈，防止背景图和按钮前景颜色相同导致按钮无法看清。如果要去掉这个灰色背景图，可以配置button的背景颜色为透明：background:'rgba(0,0,0,0)'

### 原生导航栏绘制图片

通过在titleNView里配置tags，可以实现导航栏绘制图片的效果：

```
复制代码{  
    "path" : "nav-image/nav-image",  
    "style" : {  
        "app-plus" : {  
            "titleNView" : {  
                "titleText" : "",  
                "tags" : [  
                    {  
                        "tag" : "img",  
                        "src" : "/static/nav.png",  
                        "position" : {  
                            "left" : "auto",  
                            "top" : "auto",  
                            "width" : "110px",  
                            "height" : "26px"  
                        }  
                    }  
                ]  
            }  
        }  
    }  
}
```

通过配置 tags 除了可以绘制图片，还可以绘制更多丰富的内容，如：richtext（富文本）、font（文本）、input（输入框）、rect（矩形区域）。详情参考：[titleNView](http://www.html5plus.org/doc/zh_cn/webview.html#plus.webview.WebviewTitleNViewStyles)、[tags](http://www.html5plus.org/doc/zh_cn/nativeobj.html#plus.nativeObj.ViewDrawTagStyles)。

以上代码在hello uni-app的模板-顶部导航标题栏中有示例。

### 通过setStyle方式动态修改原生导航栏样式

如果需要js动态修改导航栏，uni有跨端的api可修改标题、背景色、前景色。这部分是app、小程序、h5都支持的，参考<https://uniapp.dcloud.io/api/ui/navigationbar>。

对于app侧扩展的设置，比如自己添加的buttons，则需使用plus的js api来动态设置。在App端可以通过得到webview对象，通过setStyle方法重新设置，包括修改webview对象的titleNview属性，以达到修改标题栏按钮文字及样式的功能。
具体参考：<https://ask.dcloud.net.cn/article/35374>

### App侧使用subnvue自行绘制原生导航

nvue其实是weex上补充了uni的api。
uni-app支持使用nvue页面，也就是weex原生引擎，绘制顶部的原生导航栏。
在hello uni-app的API-界面示例中，有subnvue示例，里面顶部导航栏是渐变色的，这就是subnvue的原生导航栏。

在pages.json的配置如下：

```
复制代码{  
    "path": "subnvue/subnvue",  
    "style": {  
        "app-plus": {  
            "titleNView": false,  
            "subNVues": [{  
                "id": "nav",  
                "path": "subnvue/subnvue/nav",  
                "type": "navigationBar"  
            }]  
        }  
    }  
}
```

### App侧使用plus.nativeObj.view自定义原生导航栏

注意：从HBuilderX 1.9.10起提供了subnvue，比使用plus.nativeObj.view自定义原生导航栏更加方便。详见上一节。

titleNView提供的配置，虽然比微信多不少，但有时仍然无法满足某些场景的需求，比如在titleNView中画一个选项卡。
此时有3种处理方式。1. 使用plus.nativeObj.view的api自定义titleNView。2. 页面采用nvue，即weex方式制作。3. 取消原生导航，使用view自行绘制（见上）。
本节先说方式1. 使用plus.nativeObj.view
plus.nativeObj是5+引擎提供的轻量原生渲染引擎，其中plus.nativeObj.view一个自定义性很强的对象，以下简称nview。
规范文档是：[www.html5plus.org/doc/zh_cn/nativeobj.html#plus.nativeObj.View](http://www.html5plus.org/doc/zh_cn/nativeobj.html#plus.nativeObj.View)
nview是一个基于canvas理念的绘制引擎，在一块画布上自行绘制、覆盖、擦除。
nview可以画出任何界面、线条、矩形、文字、图片、包括原生的input输入框。
其实我们所看到的各种界面对象控件，在计算机底层都是绘图引擎基于draw字、draw图、draw线条来做的。
与weex相比，nview并不够强，nview没有dom概念，不支持内部滚动。
其实titleNView，包括原生tabbar、cover-view，他们的底层实现都是基于nview的。

获取当前页面的titleNView对象，参考<http://ask.dcloud.net.cn/article/35036>
同时上述参考文章中还有一个给titleNView的右上角画一个小红点的例子。

开发者制作的示例，如何给原生导航栏顶部画一个原生input：<http://ask.dcloud.net.cn/article/35201>

## 取消原生导航栏后，使用前端标签组件模拟绘制导航栏

不管是全局取消原生导航栏，还是在App下某个页面取消原生导航，如果还想自己绘制一些个性化的title，往往会使用view组件。
尤其是App的首页，顶部经常有各种特殊设置，此时需要自己使用前端技术来绘制导航。

导航栏应该是由状态栏和标题栏构成，状态栏的高度为 **var(--status-bar-height)** 此变量为uni-app框架提供仅在在css生效，标题栏的高度设为88px，整个状态栏的高度应为： calc(var(--status-bar-height) + 88px)
（upx主要针对宽度，高度无所谓还可以使用px）

```
复制代码.title-contents{  
    height: calc(var(--status-bar-height) + 88px);  
}  
.status{  
    height: var(--status-bar-height);  
}  
.titles{  
    height: 88px;  
}
```

状态栏和标题栏都应固定在页面顶部，需设置 position:fixed，标题栏的top应为状态栏的高度

```
复制代码.top-view{  
    width: 100%;  
    position: fixed;  
    top: 0;  
}  
.titles{  
    top: var(--status-bar-height);  
}
```

绘制的返回箭头需要绑定点击事件，返回上一个页面

```
复制代码<view class="titleLeftButton" @click="backButton"></view>  

methods:{  
    backButton(){  
        uni.navigateBack()  
    }  
}
```

以下为导航栏组件的部分代码

```
复制代码<template>  
    <view class="title-contents">  
        <view class="top-view status" :style="{background:statusColor}"></view>  
        <view class="_top titles" :style="{background:statusColor}">  
            <view class="titleLeftButton" @click="backButton" v-if="showLeftButton"></view>  
            <view class="titleText" :class="titleClass">{{titleText}}</view>  
            <view class="titleRightButton" @click="rightButton" v-if="showRightButton"></view>  
        </view>  
    </view>  
</template>  
<script>  
    export default {  
        props:{  
            titleText:{  
                type:String,  
                default:""  
            },  
            statusColor:{  
                type:String,  
                default:"#8F8F94"  
            },  
            showLeftButton:{  
                type:Boolean,  
                default:true  
            },  
            showRightButton:{  
                type:Boolean,  
                default:false  
            }  
        },  
        methods:{  
            backButton(){  
                uni.navigateBack()  
            },  
            ...  
        }  
    }  
</script>  
<style>  
    ...  
    .top-view{  
        width: 100%;  
        position: fixed;  
        top: 0;  
    }  
</style>  
```

Ps:若页面不需要标题栏，只需一个状态栏的view占位，那么只需在页面添加一个view即可不需要引入外部组件以免影响性能。

```
复制代码<view class="status-contents">  
    <view class="status top-view"></view>  
</view>
```

```
复制代码//css  
.status-contents{  
    height: var(--status-bar-height);  
}  
.top-view{  
    width: 100%;  
    position: fixed;  
    top: 0;  
}  
.status{  
    height:var(--status-bar-height);  
}
```

uni ui里有前端实现的**自定义导航栏组件**，推荐不要自己写，直接用写好的组件，<https://ext.dcloud.net.cn/plugin?id=52>，hello uni-app的uni ui中也有示例。

### 注意事项

取消原生导航栏后，自己使用HTML自定义组件模拟导航栏，会有很多性能体验问题：

1. 加载不如原生导航快
2. 下拉刷新无法从自定义的导航栏组件下面下拉
3. 必须取消页面的bounce效果，否则滚动到顶时再拖屏幕，在iOS上发现title也被拖下来了。
4. 滚动条会通顶
   所以除非不得以，不要使用全局取消原生导航栏的做法。
   如必须使用，注意如下几点：
5. 涉及到导航栏高度的css尽量放置在App.vue里面以提高渲染速度（css渲染顺序：先渲染App.vue里面的css，再渲染页面css）
6. 状态栏颜色应设置默认颜色，若非必要，不建议修改其颜色
7. 减少在组件中使用 :style="" 的使用以提高性能
8. 下拉刷新使用circle方式，并设置offset，让下拉刷新的圈从指定位置开始下拉，具体见pages.json配置文档

有个高频场景是App首页的title自定义，如果实现的效果很个性化，那么使用plus.nativeObj.view的方案会过于复杂，由于首页并不存在新页面进入立即渲染的压力，所以App首页如果要大幅定制，推荐使用前端view绘制，而不是使用plus.nativeObj.view。

如果把自定义导航封装成组件，虽然多个页面引入方便，但性能下降，因为这种自定义组件的加载是晚于页面基本元素的，会导致新页面进入动画时无法渲染title。
所以导航条这种要求在动画期渲染的东西，尽量不要使用自定义组件方式。

在hello uni-app示例中有各种导航栏的源码。
在扩展ui中有前端自定义导航栏。
在模板中有各种原生的导航栏。
大多数情况复制这些代码就够了。