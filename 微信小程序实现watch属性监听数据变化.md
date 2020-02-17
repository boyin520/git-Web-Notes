# 微信小程序实现watch属性监听数据变化


Vue 提供了一种通用的方式来观察和响应 Vue 实例上的数据变动：监听属性 watch。

虽然watch的滥用会导致性能不佳，但在一些情况下我们还是需要watch，使得代码更加简洁、逻辑更加清晰(其实就是嫌麻烦...)。

接下来我将逐步讲解微信小程序中如何实现一个监听器 watch，若想直接看最终代码，可直接滑动至底部。

监听器的原理，将data中需监听的属性写在watch对象中，并给其提供一个方法，当被监听属性的值改变时，调用该方法。​​

所以很显然，我们需要用到Javascript中的Object.defineProperty()方法，来手动劫持对象的getter/setter，从而实现给对象赋值时(调用setter)，执行watch对象中相对应的函数，达到监听效果。Object.defineProperty()不在这里详细介绍，还不会使用的童鞋速戳这里-> Object.defineProperty()介绍。

首先，既然是微信小程序自定义watch属性，我建议直接将代码写在app.js内，需要使用的页面直接在onLoad()内调用getApp().setWatch(...)即可

    // app.js
     
    /**
     * 小程序初始化
     */
    onLaunch(){...} 
     
    /**
     * 设置监听器
     */
    setWatcher(data, watch) { // 接收index.js传过来的data对象和watch对象
        Object.keys(watch).forEach(v => { // 将watch对象内的key遍历
            this.observe(data, v,watch[v]); // 监听data内的v属性，传入watch内对应函数以调用
        })
    },
     
    /**
     * 监听属性 并执行监听函数
     */
    observe(obj, key,watchFun) {
        var val = obj[key]; // 给该属性设默认值
        Object.defineProperty(obj, key, {
            configurable: true,
            enumerable: true,
            set: function(value) {
                val = value;
                watchFun(value,val); // 赋值(set)时，调用对应函数
            },
            get: function() {
                return val;
            }
        })
    }
此时在index.js中已经可以实现简单的属性监听：

//index.js
Page({
    data: {
        name:"xuyang"
    },
    onLoad(){
        getApp().setWatcher(this.data, this.watch); // 设置监听器
        this.setData({
            name:'lxm'
        })
    },
    watch:{
        name:function(newValue){
            console.log(newValue); // name改变时，调用该方法输出新值。
        }
    }
})
我们可以看到，index页面启动后，name被赋值成'lxm'，同时触发其setter，调用watch内的name()方法，控制台打印出 lxm。

是不是挺简单的呢？掌握了这一点，接下来的事情就好办多了。

我们知道，Vue中 watch对象内不光是能写name、age这类属性，还能用引号写出'my.name'、'my.age'这类属性，即对my对象下的name、age进行监听。 我们只需要改进一下setWatcher方法：

    /**
     * 设置监听器
     */
    setWatcher(data, watch) {
        Object.keys(watch).forEach(v => {
            let key = v.split('.'); // 将watch中的属性以'.'切分成数组
            let nowData = data; // 将data赋值给nowData
            for (let i = 0; i < key.length - 1; i++) { // 遍历key数组的元素，除了最后一个！
                nowData = nowData[key[i]]; // 将nowData指向它的key属性对象
            }
            let lastKey = key[key.length-1];
            // 假设key==='my.name',此时nowData===data['my']===data.my,lastKey==='name'
            this.observe(nowData, lastKey,watch[v]); // 监听nowData对象的lastKey
        })
    }
此时在index.js中可以这样写：

//index.js

Page({
    data: {
        my:{
            name:"xuyang",
            age:21
        }
    },
    onLoad(){
        getApp().setWatcher(this.data, this.watch);
        this.data.my.name = 'lxm';
        this.data.my.age = 2;
        this.setData({
            my: this.data.my
        })
    },
    watch:{
        'my.name':function(newValue){
            console.log(newValue);
        },
        'my.age':function(newValue){
            console.log(newValue);
        }
    }
})
控制台会打印出 lxm 和 2

现在我们又离成功更进了一步~ 接下来我们要实现Vue中的深度监听，即监听my对象即可监听它的内部所有属性以及属性的属性以及属性的属性的属性......并且监听函数的this要指向这个page里的this。

需要深度监听，Vue中的写法是 

watch:{
    my:{
        handler(newValue){
            // do something...
        },
        deep:true // 深度监听
    }
}
我们需要修改一下app.js中的两个函数：

    /**
     * 设置监听器
     */
    setWatcher(page) {
        let data = page.data;
        let watch = page.watch;
        Object.keys(watch).forEach(v => {
            let key = v.split('.'); // 将watch中的属性以'.'切分成数组
            let nowData = data; // 将data赋值给nowData
            for (let i = 0; i < key.length - 1; i++) { // 遍历key数组的元素，除了最后一个！
                nowData = nowData[key[i]]; // 将nowData指向它的key属性对象
            }
            let lastKey = key[key.length - 1];
            // 假设key==='my.name',此时nowData===data['my']===data.my,lastKey==='name'
            let watchFun = watch[v].handler || watch[v]; // 兼容带handler和不带handler的两种写法
            let deep = watch[v].deep; // 若未设置deep,则为undefine
            this.observe(nowData, lastKey, watchFun, deep, page); // 监听nowData对象的lastKey
        })
    },
    /**
     * 监听属性 并执行监听函数
     */
    observe(obj, key, watchFun, deep, page) {
        var val = obj[key];
        // 判断deep是true 且 val不能为空 且 typeof val==='object'（数组内数值变化也需要深度监听）
        if (deep && val != null && typeof val === 'object') { 
            Object.keys(val).forEach(childKey=>{ // 遍历val对象下的每一个key
                this.observe(val,childKey,watchFun,deep,page); // 递归调用监听函数
            })
        }
        var that = this;
        Object.defineProperty(obj, key, {
            configurable: true,
            enumerable: true,
            set: function(value) {
                // 用page对象调用,改变函数内this指向,以便this.data访问data内的属性值
                watchFun.call(page,value,val); // value是新值，val是旧值
                val = value;
                if(deep){ // 若是深度监听,重新监听该对象，以便监听其属性。
                    that.observe(obj, key, watchFun, deep, page); 
                }
            },
            get: function() {
                return val;
            }
        })
    }
其中需要特别注意，若是data中先声明一个对象userInfo = {}，再给userInfo赋值一个对象(比如从服务器获取用户数据)，则需要重新遍历监听才能实现深度监听，故需加上这段代码：

if(deep){ // 若是深度监听,重新监听该对象，以便监听其属性。
        that.observe(obj, key, watchFun, deep); 
}

此时index.js中需要深度监听的属性，可以这样写：

//index.js

Page({
    data: {
        my: {
            name: 'xuyang',
            age: 21,
            hobby: ['girls', 'games']
        },
        nameInfo:{}
    },
    onLoad() {
        getApp().setWatcher(this);
        this.data.my.hobby[0] = 'study';
        this.setData({
            nameInfo:{name:'haha',sex:'boy'}
        })
        console.log(this.data)
    },
    watch: {
        my:{
            handler(newValue) {
                console.log(newValue);
            },
            deep:true
        }
    }
})
控制台中首先输出 study ，接着输出{ name: 'haha', sex: 'boy' }，接着输出this.data的值，如下图：



可见my对象以及内部的所有属性都有setter/getter，nameInfo对象内也都有setter/getter，深度监听完成。

注：watch只能监听已存在的属性，数组的push(),pop()等方法并不会触发监听函数哦~

源码链接

克隆到本地后，将watch.js复制到项目的util下，在需要监听的页面按需引入即可：

//index.js  

const watch = require("../../utils/watch.js");

Page({
    data:{
        flag:true
    },
    onLoad(){
        watch.setWatcher(this); // 设置监听器，建议在onLoad下调用
        this.setData({flag:false})
    },
    watch:{
        flag:function(newVal,oldVal){
            console.log(newVal,oldVal);
        }
    }
})
当然，你也可以全局引入一次，但由于小程序的限制，我们还是不得不在每一个页面调用getApp().setWatcher(this);

//app.js
const watch = require("./utils/watch.js");
App({
    setWatcher(page){
        watch.setWatcher(page);
    }
})

//index.js  
Page({
    onLoad() {
        getApp().setWatcher(this);
    },
})

