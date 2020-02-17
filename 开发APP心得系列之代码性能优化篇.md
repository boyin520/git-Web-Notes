# 开发APP心得系列之代码性能优化篇

分类：HTML5+

[技术分享](https://ask.dcloud.net.cn/topic/%E6%8A%80%E6%9C%AF%E5%88%86%E4%BA%AB) [HTML5](https://ask.dcloud.net.cn/topic/HTML5) [JavaScript](https://ask.dcloud.net.cn/topic/JavaScript) [5 App开发](https://ask.dcloud.net.cn/topic/5+App%E5%BC%80%E5%8F%91)

算起来免费使用Dcloud开发APP也两年多了，踩过的坑，蹚过的水一波又一波，在此先感谢多位大大的无私帮助。
废话不多说，先来一炮代码精简、性能优化论，个人观点，不喜勿喷。

引：h5+ APP本身就已经多了层调用，所以如果代码再不进行优化精简那就真的承受不起了

[![img](https://img-cdn-qiniu.dcloud.net.cn/uploads/article/20170823/cc7de363a8b711c9bb0c3c61f6051e96.gif)](https://img-cdn-qiniu.dcloud.net.cn/uploads/article/20170823/cc7de363a8b711c9bb0c3c61f6051e96.gif)

**能不引入其他js框架绝不引入**

虽说文件是直接存储在手机本地，但是一个webview页面载入如此多的方法，需要用到的方法却少之又少。
比如很多人都喜欢直接引入jquery之类的框架，大把的方法用起来是方便了，冗余的代码却影响了APP的性能，所以建议的方法是自己扩展封装一些常用的方法，如removeClass、hasClass、getIndex、siblings、selectValue等，这些方法再配合mui.js框架基本能满足页面交互需求，还有满足不了的那就原生js处理（当然有现成js插件肯定优先，但首选原生js插件）。

```
复制代码 //获取兄弟元素  
app.siblings=function(el,childEl) {  
    var r = [],n;  
    if(!childEl){  
        n = el.parentNode.children;  
    }else{  
        n = el.parentNode.querySelectorAll(childEl);  
        }  
    for(var i =0,pl= n.length;i<pl;i  ) {  
        if(n[i] !== el){ r.push(n[i]);}  
    }  
    return r;  
}  

//获取索引  
app.getIndex=function(el){  
    var child=el.parentNode.children;  
    for(var i=0; i<child.length; i  ){  
        if(el==child[i])  
            return i;  
    }  
}  
//是否包含某个class  
app.hasClass = function(el,name) {  
    return (el.className.indexOf(name)>=0);  
}
```

另外模块化、MVVM基本不太适用于开发h5 APP，因为都是单页面，一个页面引入的文件就那么三两个，再加入模块化反而显得复杂了，vuejs、ng之类的同样发挥不出他们的优势，反而增加冗余，页面的渲染引入轻量级js模板引擎拼接即可，如artTemplate（template.helper过滤器处理复杂数据）。

**封装常用方法便于重复使用**

APP里面用得最多的可能就是openWindow方法，如果每个地方都把这个方法copy一遍那整体增加的行数就多了，所以建议进行些简单的封装，如：

```
复制代码//打开新窗口  
app.openWV=function(id,extras,url){  
    mui.openWindow({  
        id:id,  
        url:url||id,  
        extras:extras,  
        show: {  
            autoShow:true,  
            aniShow: 'pop-in',  
            duration:300  
        },  
        waiting: {  
            autoShow: false  
        }  
    });  
}  
app.openWV('order-detial.html',parmas);
```

看代码应该会发现url是可以不传的，因为这里是直接将页面的url作为了webview的id来使用，这样使用的好处是直接通过页面文件名称获取webview，清晰明了，为保证url无需在文件中跳来跳去建议所有文件都放在根目录，使用一定的命名规范整理，如user相关页面均为user-name.html，order相关 order-name.html，这样url基本就用不上，也无需考虑文件夹层级关系。

其他常用的方法还有timeFormat（时间格式化）、

```
复制代码app.timeFormat=function(time,params){  
    var d=time?new Date(time):new Date(),  
        year=d.getFullYear(),  
        month=d.getMonth() 1,  
        day=d.getDate(),  
        hours=d.getHours(),  
        minutes=d.getMinutes(),  
        seconds=d.getSeconds();  

    if(month<10) month='0' month;  
    if(day<10) day='0' day;  
    if(hours<10) hours='0' hours;  
    if(minutes<10) minutes='0' minutes;  
    if(seconds<10) seconds='0' seconds;  

    if(params){  
        return {year:year,month:month,day:day};  
    }else{  
        return (year '-' month '-' day ' ' hours ':' minutes ':' seconds);  
    }  
}
```

parseDomImg（格式化详情页图片为占位图，用于实现图片延迟加载）、

```
复制代码app.parseDomImg=function(str){  
    var objE = document.createElement("div");  
    objE.innerHTML = str;  

    var imgs=objE.querySelectorAll('img');  
    for(var i=0; i<imgs.length; i  ){  
        var img=imgs[i];  
        img.setAttribute('data-delay',img.src);  
        img.src='images/blank.gif';  
        img.setAttribute('height','100px');  
        img.setAttribute('width','100%');  
    }  
    return objE.innerHTML;  
};
```

showLogin(params)（可在params对象中传递登录后回调需要的参数）、

```
复制代码app.showLogin=function(params){  
    app.openWV('login.html',{params:params});   
//此处params为json对象，可在login页面判断isEmptyObject(params)，进行回调执行登录前用户操作  
};
```

isLogin（判断是否登录）、loadShare（加载分享功能）、reloadWV（请求失败加载重新载入提示及方法）等等等。。。

**遵循能少操作DOM就尽量简化、能监听不批量绑定事件的原则**

当我们频繁获取、操作DOM对象时消耗的资源肯定比操作内存中的变量要多得多，所以建议将频繁操作对象缓存到内存中，如某个结构内容需要每次交互去改变，那就把他缓存起来，var List = document.getElementById('List'); 后续通过List变量去操作，而不是每次获取。

for循环往DOM中插入新获取的数据列表那就更加不可取了，1w条数据就会插入1w次，这让DOM怎么“承受”，每次发生几何变化，页面都会进行重排、重绘，合理的方式是通过template或者其他方式拼接好需要新增的列表

```
复制代码List.insertAdjacentHTML('beforeend', template('initData', data));  //insertAdjacentHTML方法可以实现任意位置插入，用法自行百度
```

关于事件绑定，如果一块区域多个元素需要绑定同一个或者不同方法，那么千万不要for循环去给每个元素绑定方法或多个方法绑定，应使用on方法监听初始已存在父级元素的点击事件，一次监听解决千万次绑定。

```
复制代码mui('#List').on('tap', 'li', function() {  
    app.openWV(this.getAttribute('data-id'));                 
});  
mui('#parentBox').on('tap', '.child', function() {  
        var type=this.id;  
    switch(type){  
        case 'del':  
        break;  
        case 'save':  
        break;  
    }             
});
```

**遵循能用css解决的问题不用js、图片处理的原则**

能用一句addClass active解决整个列表多个元素的特殊处理的不要用for循环去重复操作，能添加一个class执行动画的不用js去处理。
能用css轻松写出来的效果不要使用图片或js实现，活用css3。
如切换至待付款状态订单需要提供checkbox批量支付，就只需要给列表容器添加isPay class显示出来即可；

**合理利用预加载**

虽然整个APP同时存在的webview是有限制的，但是频繁创建webview更是要消耗资源的，所以对于常用页面使用预加载处理，如详情页，初始预加载基本结构，每次需要打开时通过fire方法去重新loadData打开即可，如筛选页面，不同的页面可能需要展示不同的筛选条件、不同的筛选规则，那么也预加载他，每次打开时传递对应的handle参数判断规则，判断是否需要reset选中。

```
复制代码//操作页  
var filterParam={};  
mui.extend({  
    pageno: page,  
    pagesize: 20,  
    themeid: themeid,  
    userkey: userkey,  
    type: type,  
    isasc:isAsc  
},filterParam)   //拼接筛选页返回参数json  

filterBtn.addEventListener('tap', function() {  
    showFilter(ws.id,isEmptyObject(filterParam),'isGoods');  
});   // 打开筛选页面，传递当前页面id，初始filterParam为空reset选中，handle特殊处理规则  

document.addEventListener('updateFilter',function(e){  
    filterParam=e.detail.filterParam;  
    reloadPage();  
});  

//筛选页  
var checkArr={};  

if(handle=='isGoods'){.....}  
if(isReset){.....}  

mui.fire(plus.webview.getWebviewById(openUrl),'updateFilter',{filterParam:checkArr});  
mui.back();
```

经测试iPhone 6plus手机，重复操作打开商品详情---关闭--打开（商品详情包含大量的图片，非预加载），循环50次左右，问题就来了，商品详情页花屏了，渲染不出完整的页面了，只能重启应用。所以，常用页面建议使用预加载避免重复创建销毁。

**合理处理图片延迟加载、合理使用分页加载、避免同时发起多个请求**

移动端图片加载问题是很突出的，同时请求多个图片拖缓了重要内容的加载，所以延迟加载图片是非常必要的，当然控制图片大小也是基础，这里提供一个自己写的一个原生js的图片延迟加载插件，简单粗糙，但是实用，支持背景图片及图片元素渐入延迟加载、快速滑动停留超过N毫秒加载、指定div容器滑动加载、追加新增图片等。https://github.com/xielingxiao/delayimg

```
复制代码delayimg.init();   //初始化 （可混用图片元素、背景图片）  
delayimg.render();  // 追加图片方法
```

超过20条数据（具体看数据量）的列表最好进行分页加载，同时请求大量数据不仅让服务器查询耗时，客户端处理也需要时间，用户体验极度不好，所以分屏分页加载数据很有必要。

提供加载提示、渐入式展现页面，打开新页面提供菊花等待或空页面放入mui-icon mui-spinner菊花元素，待ajax请求到数据后替换。如果页面初始存在一些错乱或者不美观的结构，那么建议初始状态给mui-content添加.transparent{ opacity: 0;}透明处理，配合css3动画，数据插入DOM后移除透明渐入，给用户展现流畅协调的页面。

避免同时发起多个请求，最常见的场景就是父子页面加载问题（多子页面），如果一次性创建所有子页面并且加载子页面的数据那速度肯定慢了，所以index为入口页面，先插入多个子页面空壳，除home第一个子页面自动请求数据外，其他子页面默认不加载内容，只添加加载监听方法，用户操作点击时再加载内容。对于需要每次切入都需要更新数据的页面再监听其他的更新数据方法。

```
复制代码//index页面  
mui('.mui-bar-tab').on('tap','a',function(){  
    if(href!='home.html'){  
        mui.fire(plus.webview.getWebviewById(this.getAttribute('data-href')),'loadData');  
    }  
});  

//子页面  
var flag=false;  
document.addEventListener('loadData', function() {  
    if(!flag){  
               flag=true;  
               mui.init();  
               .....  // 执行页面数据加载等方法  
        }  
});
```

**规范化你的代码**

代码的规范在开发中是重中之重，有语意有规范的代码不仅方便自己，也不会困扰与你协作的‘同志’。
精简代码，能复用扩展的绝不滥用copy，统一的缩进、统一的命名规律、统一的编写方式，代码才能一目了然，具体的规范问题就不在这里详列了，可参考网上提供的。

好了，这篇就到此吧。。。暂时没想起更多，想起再补充吧。。。

已加载完....

**更多分享预告：**

[轻松调试解决问题篇（已分享）](http://ask.dcloud.net.cn/article/12745)

[接口安全加密篇（已分享）](http://ask.dcloud.net.cn/article/12744)

各种‘坑’解决方案汇总篇（持续单个问题更新）