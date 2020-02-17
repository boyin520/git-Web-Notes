# [vue全家桶](https://segmentfault.com/a/1190000019350009)

> vue全家桶知识记录,只做记录不深入原理。

`官网都有为什么还要再写一遍？`
`为了熟悉语法并且每个知识点尽量用一个简单例子说明用法`

# 1.vue基础知识

## 1.1钩子

`一个实例从创建到销毁，在每个特定的时间都会自动触发某个函数，这种函数就叫做生命周期函数，也叫钩子，vue实例有哪些钩子呢？`

```
//组件实例刚被创建，组件属性计算之前，如data属性等
beforeCreate
//组件实例创建完成，属性已绑定，但DOM还未生成，$el属性还不存在
created
//模板编译 / 挂载之前
beforeMount
//模板编译 / 挂载之后
mounted
//组件更新之前
beforeUpdate
//组件更新之后
updated
//组件被激活时调用
activated
//组件被移除时调用
deactivated
//组件销毁前调用
beforeDestory
//组件销毁后调用
destoryed
```

`每个钩子的触发时间是什么？网上找了个图：`

![gouzi](https://segmentfault.com/img/remote/1460000008926240?w=1200&h=2800)

`写点代码测试一下：`

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>index</title>
    <script src="https://cdn.bootcss.com/vue/2.6.10/vue.min.js"></script>
</head>
<body>
    
    <div id="app">{{msg}}</div>

    <script>
        var app = new Vue({
            el:'#app',
            data:{
                msg:'hello'
            },
            beforeCreate(){
      console.group('beforeCreate 创建前状态 ------------>');
      console.log("%c%s", "color:red" , "el     : " + this.$el); //undefined
      console.log("%c%s", "color:red","data   : " + this.$data); //undefined
      console.log("%c%s", "color:red","message: " + this.message)
    },
    created() {
      console.group('created 创建完毕状态 ------------>');
      console.log("%c%s", "color:red","el     : " + this.$el); //undefined
      console.log("%c%s", "color:red","data   : " + this.$data); //已被初始化
      console.log("%c%s", "color:red","message: " + this.message); //已被初始化
    },
    beforeMount() {
      console.group('beforeMount 挂载前状态 ------------>');
      console.log("%c%s", "color:red","el     : " + (this.$el)); //已被初始化
      console.log("%c%s", "color:red","data   : " + this.$data); //已被初始化
      console.log("%c%s", "color:red","message: " + this.message); //已被初始化
    },
    mounted() {
      console.group('mounted 挂载结束状态 ------------>');
      console.log("%c%s", "color:red","el     : " + this.$el); //已被初始化
      console.log(this.$el);
      console.log("%c%s", "color:red","data   : " + this.$data); //已被初始化
      console.log("%c%s", "color:red","message: " + this.message); //已被初始化
    },
    beforeUpdate() {
      console.group('beforeUpdate 更新前状态 ------------>');
      console.log("%c%s", "color:red","el     : " + this.$el);
      console.log(this.$el);
      console.log('真实dom结构：' + document.getElementById('app').innerHTML);
      console.log("%c%s", "color:red","data   : " + this.$data);
      console.log("%c%s", "color:red","message: " + this.message);
    },
    updated() {
      console.group('updated 更新完成状态 ------------>');
      console.log("%c%s", "color:red","el     : " + this.$el);
      console.log(this.$el);
      console.log('真实dom结构：' + document.getElementById('app').innerHTML);
      console.log("%c%s", "color:red","data   : " + this.$data);
      console.log("%c%s", "color:red","message: " + this.message);
    },
    beforeDestroy() {
      console.group('beforeDestroy 销毁前状态 ------------>');
      console.log("%c%s", "color:red","el     : " + this.$el);
      console.log(this.$el);
      console.log("%c%s", "color:red","data   : " + this.$data);
      console.log("%c%s", "color:red","message: " + this.message);
    },
    destroyed() {
      console.group('destroyed 销毁完成状态 ------------>');
      console.log("%c%s", "color:red","el     : " + this.$el);
      console.log(this.$el);
      console.log("%c%s", "color:red","data   : " + this.$data);
      console.log("%c%s", "color:red","message: " + this.message)
    }
        })


    </script>

</body>
</html>
```

![gouzz](https://segmentfault.com/img/remote/1460000019350012)

## 1.2组件

`在官网，vue组件的知识比较分散，这里把它集合起来，包括组件基础，组件传值，动态/异步组件，首先是组件基础，组件可以分为局部组件和全局组件，，具体写法如下：`

### 1.2.1全局组件

```
//全局组件
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>index</title>
    <script src="https://cdn.bootcss.com/vue/2.6.10/vue.min.js"></script>
</head>
<body>
    
    <div id="app">
        <button-counter></button-counter>
    </div>
    <script>
    Vue.component('button-counter',{
        template:`<button @click="count++">You clicked me {{ count }} times.</button>`,
        data:function(){
            return {
                count:0
            }
        }
    })
   var app = new Vue({
        el:'#app'

    })
    </script>
</body>
</html>
```

`全局注册的行为必须在根 Vue 实例 ，模板template中只能有一个根元素`

### 1.2.2局部组件

```
//局部组件
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>index</title>
    <script src="https://cdn.bootcss.com/vue/2.6.10/vue.min.js"></script>
</head>
<body>
    
    <div id="app">
        <button-counter></button-counter>
    </div>
    <script>
    var buttonCounter = {
        template:`<button @click="count++">You clicked me {{ count }} times.</button>`,
        data:function(){
            return {
                count:0
            }
        }
    }
        var app = new Vue({
            el:'#app',
            components:{
                buttonCounter
            }

        })


    </script>

</body>
</html>
```

### 1.2.3父子组件传值

`接着是组件传值，分成父子组件传值，子父组件传值，同级组件传值和复杂组件传值，首先来看父子组件传值：`

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>index</title>
    <script src="https://cdn.bootcss.com/vue/2.6.10/vue.min.js"></script>
</head>
<body>
    
    <div id="app">
        <info :msg="msg"></info>
    </div>
    <script>
        Vue.component('info',{
            template:`<div>{{this.msg}}</div>`,
            props:['msg']
        })
        var app = new Vue({
            el:'#app',
            data:{
                msg:'I am from father component'
            }
        })


    </script>

</body>
</html>
```

### 1.2.4子父组件传值

`接着是子父组件传值：`

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>index</title>
    <script src="https://cdn.bootcss.com/vue/2.6.10/vue.min.js"></script>
</head>
<body>
    
    <div id="app">
        <info @child="accept"></info>
    </div>
    <script>
        Vue.component('info',{
            template:`
                <div>
                    <button @click="send">click</button>
                </div>
            `,
            data(){
                return{
                    msg:'I am from child component'
                }
            },
            methods:{
                send:function(){
                    this.$emit('child',this.msg)
                }
            }
        })
        var app = new Vue({
            el:'#app',
            methods:{
                accept:function(msg){
                    alert(msg)
                }
            }
        })


    </script>

</body>
</html>
```

### 1.2.5同级组件传值

`再来写同级传值：`

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>index</title>
    <script src="https://cdn.bootcss.com/vue/2.6.10/vue.min.js"></script>
</head>
<body>
    
    <div id="app">
        <info @child="accept"></info>
        <msg :value="value"></msg>
    </div>
    <script>
        Vue.component('info',{
            template:`
                <div>
                    <button @click="send">click</button>
                </div>
            `,
            data(){
                return{
                    msg:'I am from info component'
                }
            },
            methods:{
                send:function(){
                    this.$emit('child',this.msg)
                }
            }
        })

        Vue.component('msg',{
            template:`
                <div>
                    {{this.value}}
                </div>
            `,
            props:['value']

        })
        var app = new Vue({
            el:'#app',
            data:{
                value:''
            },
            methods:{
                accept:function(msg){
                    this.value = msg;
                }
            }
        })


    </script>

</body>
</html>
```

`至于说复杂传值是通过vuex状态管理器来实现的，这个放在后面说`

### 1.2.6动态组件

`动态组件就是通过is属性动态的切换组件，它是为了实现让多个组件使用同一个挂载点，并动态切换,动态切换的组件是被移除掉了，如果把切换出去的组件保留在内存中，可以保留它的状态或避免重新渲染,这就需要keepalive来保存`

```
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Vue 测试实例 - 动态组件</title>
<script src="https://cdn.bootcss.com/vue/2.2.2/vue.min.js"></script>
</head>
<body>
<div id="app">
    <button @click='toShow'>点击显示子组件</button>
    <keep-alive>
    <component v-bind:is="which_to_show" ></component>
    </keep-alive>
</div>
 
<script>
 
var vm = new Vue({  
        el: '#app',  
        data: {  
            which_to_show: "first"  
        },  
        methods: {  
            toShow: function () {   //切换组件显示  
                var arr = ["first", "second", "third", ""];  
                var index = arr.indexOf(this.which_to_show);  
                if (index < 2) {  
                    this.which_to_show = arr[index + 1];  
                } else {  
                    this.which_to_show = arr[0];  
                }  
               console.log(this.$children);  
            }  
        },  
        components: {  
            first: { //第一个子组件  
                template: "<div>这里是子组件1</div>"  
            },  
            second: { //第二个子组件  
                template: "<div>这里是子组件2，这里是延迟后的内容：{{hello}}</div>",  
                data: function () {  
                    return {  
                        hello: ""  
                    }  
                },  
                activated: function (done) { //执行这个参数时，才会切换组件  
                    var self = this; 
                                        var startTime = new Date().getTime(); // get the current time   
                                        //两秒后执行
                                        while (new Date().getTime() < startTime + 2000){
                                            self.hello='我是延迟后的内容';
                                        }
                    
                }  
            },  
            third: { //第三个子组件  
                template: "<div>这里是子组件3</div>"  
            }  
        }  
    });  
</script>
</body>
</html>
```

`初始情况下，vm.$children属性中只有一个元素（first组件），点击按钮切换后，vm.$children属性中有两个元素，再次切换后，则有三个元素（三个子组件都保留在内存中）。之后无论如何切换，将一直保持有三个元素。`
![ssd](https://segmentfault.com/img/remote/1460000019350013)

### 1.2.7异步组件

`异步组件：将应用分割成小一些的代码块,只在需要的时候才从服务器加载一个模块`

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>异步组件</title>
    <script src="https://cdn.bootcss.com/vue/2.6.10/vue.js"></script>
</head>
<body>
    <div id="app">
        <async></async>
    </div>
<script>
    Vue.component('async', function (resolve, reject) {
      setTimeout(function () {
        resolve({
          template: '<div>I am async!</div>'
        })
      }, 1000)
    })
    new Vue({
      el: '#app'
})
    
</script>
</body>
</html>
```

![ssfg](https://segmentfault.com/img/remote/1460000019350014)

## 1.3插槽

`插槽：组件插入内容和组件内部的联通,有基本插槽，具名插槽和作用域插槽`

### 1.3.1基本插槽

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>插槽</title>
    <script src="https://cdn.bootcss.com/vue/2.6.10/vue.js"></script>
</head>
<body>
    <div id="app">
        <me-component>
            <h1>我是header</h1>  
        </me-component>
    </div>
<script >
    Vue.component('me-component', {         
        template:
                `<div>
                    <p>你好</p>
                    <slot></slot> 
                </div>`
    })
    var vm = new Vue({
        el:"#app"
    })
</script >
</body>
</html>
```

### 1.3.2具名插槽

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>插槽</title>
    <script src="https://cdn.bootcss.com/vue/2.6.10/vue.js"></script>
</head>
<body>
    <div id="app">
        <me-component>
            <h2 slot="footer">插槽内容</h2>
        </me-component>
    </div>
<script >
    Vue.component('me-component', {         
        template:
                `<div>
                    <p>模板内容</p>
                    <slot name="footer"></slot> 
                </div>`
    })
    var vm = new Vue({
        el:"#app"
    })
</body>
</html>
```

### 1.3.3作用域插槽

`作用域插槽：组件擦汗日内容接收到组件内部的数据`

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>作用域插槽</title>
    <script src="https://cdn.bootcss.com/vue/2.6.10/vue.min.js"></script>
</head>
<body>
        <div id="app">
            <me>
                <template v-slot:default="slotProps">
                    <div>{{slotProps.user.firstName}}</div>
                </template>
            </me>
        </div>


        <script>
                Vue.component('me',{
                    template:`<div>
                        <slot :user="user">hello</slot>
                    </div>`,
                    data(){
                        return {
                            user:{
                                firstName:'kk',
                                lastNmae:'G'
                            }
                        }
                    }
                })
                var app = new Vue({
                    el:'#app'
                })

        </script>
</body>
</html>
```

## 1.4过渡动画

### 1.4.1过渡

`为状态发生改变的内容添加动态效果`

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>过渡</title>
    <script src="https://cdn.bootcss.com/vue/2.6.10/vue.min.js"></script>
    <style>
            .fade-enter-active, .fade-leave-active {
                  transition: opacity .5s;
                }
                .fade-enter, .fade-leave-to {
                  opacity: 0;
                }
    </style>
</head>
<body>
        <div id="demo">
          <button v-on:click="show = !show">
            Toggle
          </button>
          <transition name="fade">
            <p v-if="show">hello</p>
          </transition>
        </div>


        <script>
                new Vue({
                  el: '#demo',
                  data: {
                    show: true
                  }
                })

        </script>
</body>
</html>
```

![kiuh](https://segmentfault.com/img/remote/1460000009208297?w=1078&h=430)

### 1.4.2css动画

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>过渡</title>
    <script src="https://cdn.bootcss.com/vue/2.6.10/vue.min.js"></script>
    <link rel="stylesheet" href="https://cdn.bootcss.com/animate.css/3.7.0/animate.min.css">
    <style>
            .fade-enter-active, .fade-leave-active {
                  transition: opacity .5s;
                }
                .fade-enter, .fade-leave-to {
                  opacity: 0;
                }
    </style>
</head>
<body>
        <div id="example-3">
          <button @click="show = !show">
            Toggle render
          </button>
          <transition
            name="custom-classes-transition"
            enter-active-class="animated tada"
            leave-active-class="animated bounceOutRight"
          >
            <p v-if="show">hello</p>
          </transition>
        </div>


        <script>
                new Vue({
                  el: '#example-3',
                  data: {
                    show: true
                  }
                })
        </script>
</body>
</html>
```

### 1.4.3js动画

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>过渡</title>
    <script src="https://cdn.bootcss.com/vue/2.6.10/vue.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/velocity/1.2.3/velocity.min.js"></script>
</head>
<body>
        <div id="example-4">
          <button @click="show = !show">
            Toggle
          </button>
          <transition
            v-on:before-enter="beforeEnter"
            v-on:enter="enter"
            v-on:leave="leave"
            v-bind:css="false"
          >
            <p v-if="show">
              Demo
            </p>
          </transition>
        </div>


        <script>
                new Vue({
                      el: '#example-4',
                      data: {
                        show: false
                      },
                      methods: {
                        beforeEnter: function (el) {
                          el.style.opacity = 0
                          el.style.transformOrigin = 'left'
                        },
                        enter: function (el, done) {
                          Velocity(el, { opacity: 1, fontSize: '1.4em' }, { duration: 300 })
                          Velocity(el, { fontSize: '1em' }, { complete: done })
                        },
                        leave: function (el, done) {
                          Velocity(el, { translateX: '15px', rotateZ: '50deg' }, { duration: 600 })
                          Velocity(el, { rotateZ: '100deg' }, { loop: 2 })
                          Velocity(el, {
                            rotateZ: '45deg',
                            translateY: '30px',
                            translateX: '30px',
                            opacity: 0
                          }, { complete: done })
                        }
                      }
                    })
        </script>
</body>
</html>
```

### 1.4.4多组件过渡

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>过渡</title>
    <script src="https://cdn.bootcss.com/vue/2.6.10/vue.min.js"></script>
    <style>
        .component-fade-enter-active, .component-fade-leave-active {
              transition: opacity .3s ease;
            }
            .component-fade-enter, .component-fade-leave-to {
              opacity: 0;
            }
    </style>
</head>
<body>
        <div id="transition-components-demo">
            A:<input 
                    type="radio" 
                    :checked="aIsChecked" 
                    @click="handleClick"
                >
            B:<input 
                    type="radio"
                    :checked="bIsChecked"
                    @click="handleClick"
                >

          <transition name="component-fade" mode="out-in">
              <component v-bind:is="view"></component>
            </transition>
        </div>


        <script>
                new Vue({
                  el: '#transition-components-demo',
                  data: {
                    view: 'v-a',
                    aIsChecked:true,
                    bIsChecked:false
                  },
                  components: {
                    'v-a': {
                      template: '<div>Component A</div>'
                    },
                    'v-b': {
                      template: '<div>Component B</div>'
                    }
                  },
                  methods:{
                      handleClick:function(){
                              this.aIsChecked = !this.aIsChecked;
                              this.bIsChecked = !this.bIsChecked;
                              this.view = this.view === 'v-a' ? 'v-b' : 'v-a';
                      }
                  }
                })
        </script>
</body>
</html>
```

### 1.4.5列表过渡

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>列表过渡</title>
    <script src="https://cdn.bootcss.com/vue/2.6.10/vue.min.js"></script>
    <style>
        .list-item {
          display: inline-block;
          margin-right: 10px;
        }
        .list-enter-active, .list-leave-active {
          transition: all 1s;
        }
        .list-enter, .list-leave-to{
          opacity: 0;
          transform: translateY(30px);
        }
    </style>
</head>
<body>
        <div id="list-demo" class="demo">
          <button v-on:click="add">Add</button>
          <button v-on:click="remove">Remove</button>
          <transition-group name="list" tag="p">
            <span v-for="item in items" v-bind:key="item" class="list-item">
              {{ item }}
            </span>
          </transition-group>
        </div>


        <script>
                new Vue({
                  el: '#list-demo',
                  data: {
                    items: [1,2,3,4,5,6,7,8,9],
                    nextNum: 10
                  },
                  methods: {
                    randomIndex: function () {
                      return Math.floor(Math.random() * this.items.length)
                    },
                    add: function () {
                      this.items.splice(this.randomIndex(), 0, this.nextNum++)
                    },
                    remove: function () {
                      this.items.splice(this.randomIndex(), 1)
                    },
                  }
                })
        </script>
</body>
</html>
```

### 1.4.6状态过渡

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>列表过渡</title>
    <script src="https://cdn.bootcss.com/vue/2.6.10/vue.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/tween.js@16.3.4"></script>
    <script src="https://cdn.jsdelivr.net/npm/color-js@1.0.3"></script>
    <style>
        .example-7-color-preview {
          display: inline-block;
          width: 50px;
          height: 50px;
        }
    </style>
</head>
<body>
        <div id="example-7">
          <input
            v-model="colorQuery"
            v-on:keyup.enter="updateColor"
            placeholder="Enter a color"
          >
          <button v-on:click="updateColor">Update</button>
          <p>Preview:</p>
          <span
            v-bind:style="{ backgroundColor: tweenedCSSColor }"
            class="example-7-color-preview"
          ></span>
          <p>{{ tweenedCSSColor }}</p>
        </div>

        <script>
                var Color = net.brehaut.Color

                new Vue({
                  el: '#example-7',
                  data: {
                    colorQuery: '',
                    color: {
                      red: 0,
                      green: 0,
                      blue: 0,
                      alpha: 1
                    },
                    tweenedColor: {}
                  },
                  created: function () {
                    this.tweenedColor = Object.assign({}, this.color)
                  },
                  watch: {
                    color: function () {
                      function animate () {
                        if (TWEEN.update()) {
                          requestAnimationFrame(animate)
                        }
                      }

                      new TWEEN.Tween(this.tweenedColor)
                        .to(this.color, 750)
                        .start()

                      animate()
                    }
                  },
                  computed: {
                    tweenedCSSColor: function () {
                      return new Color({
                        red: this.tweenedColor.red,
                        green: this.tweenedColor.green,
                        blue: this.tweenedColor.blue,
                        alpha: this.tweenedColor.alpha
                      }).toCSS()
                    }
                  },
                  methods: {
                    updateColor: function () {
                      this.color = new Color(this.colorQuery).toRGB()
                      this.colorQuery = ''
                    }
                  }
                })
        </script>
</body>
</html>
```

## 1.5自定义指令

### 1.5.1自定义指令参数

```
//指令参数
el: 指令所绑定的元素，可以用来直接操作 DOM 。
binding: 一个对象，包含以下属性：
    name: 指令名，不包括 v- 前缀。
    value: 指令的绑定值， 例如： v-my-directive="1 + 1", value 的值是 2。
    oldValue: 指令绑定的前一个值，仅在 update 和 componentUpdated 钩子中可用。无论值是否改变都可用。
    expression: 绑定值的字符串形式。 例如 v-my-directive="1 + 1" ， expression 的值是 "1 + 1"。
    arg: 传给指令的参数。例如 v-my-directive:foo， arg 的值是 "foo"。
    modifiers: 一个包含修饰符的对象。 例如： v-my-directive.foo.bar, 修饰符对象 modifiers 的值是 { foo: true, bar: true }。
vnode: Vue 编译生成的虚拟节点。
oldVnode: 上一个虚拟节点，仅在 update 和 componentUpdated 钩子中可用。
```

```
//指令钩子
bind: 只调用一次，指令第一次绑定到元素时调用，用这个钩子函数可以定义一个在绑定时执行一次的初始化动作。

inserted: 被绑定元素插入父节点时调用（父节点存在即可调用，不必存在于 document 中）。

update: 被绑定元素所在的模板更新时调用，而不论绑定值是否变化。通过比较更新前后的绑定值，可以忽略不必要的模板更新（详细的钩子函数参数见下）。

componentUpdated: 被绑定元素所在模板完成一次更新周期时调用。

unbind: 只调用一次， 指令与元素解绑时调用。
```

### 1.5.2自定义指令实例

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>列表过渡</title>
    <script src="https://cdn.bootcss.com/vue/2.6.10/vue.min.js"></script>
</head>
<body>
        <div id="app">
            <div id="hook-arguments-example" v-demo:foo.a.b="message"></div>
        </div>

        <script>
            Vue.directive('demo', {
              bind: function (el, binding, vnode) {
                var s = JSON.stringify
                el.innerHTML =
                  'name: '       + s(binding.name) + '<br>' +
                  'value: '      + s(binding.value) + '<br>' +
                  'expression: ' + s(binding.expression) + '<br>' +
                  'argument: '   + s(binding.arg) + '<br>' +
                  'modifiers: '  + s(binding.modifiers) + '<br>' +
                  'vnode keys: ' + Object.keys(vnode).join(', ')
              }
            })
        new Vue({
          el: '#hook-arguments-example',
          data: {
            message: 'hello!'
          }
        })
        </script>
</body>
</html>
```

## 1.6混入

`混入会将公用的组件配置项与默认配置项目混合，具体看例子：`

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>混入</title>
    <script src="https://cdn.bootcss.com/vue/2.6.10/vue.min.js"></script>
</head>
<body>
        <div id="app">
            <mixinsComponent></mixinsComponent>
        </div>

        <script>
            var myMixin = {
              created: function () {
                this.hello()
              },
              methods: {
                hello: function () {
                  console.log('hello from mixin!')
                }
              }
            }
            // 定义一个使用混入对象的组件
            var Component = Vue.extend({
              mixins: [myMixin]
            })

            var mixinsComponent = new Component()
            var app = new Vue({
                el:'#app',
                component:{
                    mixinsComponent
                }
            })
        </script>
</body>
</html>
```

## 1.7渲染函数

`vue默认使用html的模板然后绑定在虚拟dom，但还提供了渲染函数实现更加强大的原生js使用能力`

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>渲染函数</title>
    <script src="https://cdn.bootcss.com/vue/2.6.10/vue.js"></script>
</head>
<body>
    <div id="app">
        <test :test="1"></test>
    </div>


    <script>

        Vue.component('test',{
            render:function(createElement){
                return createElement('div',{
                    class:{
                        test:true
                    },
                    id:{
                        test:true
                    },
                    style:{
                        color:'green'
                    },
                    props:{
                        test:"test"
                    },
                    domProps: {
                        innerHTML: 'hello world'
                    },
                    on: {
                        click: function(){
                            alert('click event')
                        }
                    }
                })
            }
        })
        var app = new Vue({
            el:'#app'
        })
    </script>
</body>
</html>
```

## 1.8插件开发

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>插件</title>
    <script src="https://cdn.bootcss.com/vue/2.6.10/vue.js"></script>
</head>
<body>
    <div id="app">
    </div>


    <script>
        var Test = {}
        Test.install = function(Vue,options){
            Vue.globalVarible = 'hello world';
            Vue.mixin({
                mounted:function(){
                    alert(this.globalVarible)
                }
            })
        };
        Vue.use(Test);
        var app = new Vue({
            el:'#app'
        })
    </script>
</body>
</html>
```

# 2.vue-router基础知识

## 2.1基本路由

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>路由</title>
    <script src="https://unpkg.com/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
</head>
<body>
    <div id="app">
        <router-link to="/foo">Go to Foo</router-link>
          <router-view></router-view>
    </div>


    <script>
        var router = new VueRouter({
            routes:[
                {
                    path:'/foo',
                    component:{
                        template:`<div>foo</div>`
                    }
                },
                {
                    path:'/',
                    component:{
                        template:`<div>index</div>`
                    }
                }
            ]
        })

        var app = new Vue({
            el:'#app',
            router:router
        })
    </script>
</body>
</html>
```

## 2.2动态路由

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>路由</title>
    <script src="https://unpkg.com/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
</head>
<body>
    <div id="app">
        <router-link to="/foo/one">Go to Foo one</router-link>
        <router-link to="/foo/two">Go to Foo two</router-link>

          <router-view></router-view>
    </div>


    <script>
        var router = new VueRouter({
            routes:[
                {
                    path:'/foo/:name',
                    component:{
                        template:`<div>{{$route.params.name}}</div>`
                    }
                },
                {
                    path:'/',
                    component:{
                        template:`<div>index</div>`
                    }
                }
            ]
        })

        var app = new Vue({
            el:'#app',
            router:router
        })
    </script>
</body>
</html>
```

## 2.3嵌套路由

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>路由</title>
    <script src="https://unpkg.com/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
</head>
<body>
    <div id="app">
        <router-link to="/">index</router-link>

          <router-view></router-view>
    </div>


    <script>
        var router = new VueRouter({
            routes:[
                {
                    path:'/',
                    component:{
                        template:`
                            <div>
                                 <div>index</div>
                            <router-link to="bar" append>bar</router-link>
                              <router-view></router-view>
                            </div>
                        `
                    },
                    children:[
                        {
                            path:'bar',
                            component:{
                                template:`<div>hello</div>`
                            }
                        }
                    ]
                }
            ]
        })

        var app = new Vue({
            el:'#app',
            router:router
        })
    </script>
</body>
</html>
```

## 2.4编程式导航

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>路由</title>
    <script src="https://unpkg.com/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
</head>
<body>
    <div id="app">
        <router-link to="/">index</router-link>
        <router-link to="/list">list</router-link>
        <router-link to="/detail">detail</router-link>

          <router-view></router-view>
          <button @click="change">导航</button>
    </div>


    <script>
        var router = new VueRouter({
            routes:[
                {
                    path:'/',
                    component:{
                        template:`
                            <div>
                                 index
                            </div>
                        `
                    }
                },
                {
                    path:'/list',
                    component:{
                        template:`
                            <div>
                                 list
                            </div>
                        `
                    }
                },
                {
                    path:'/detail',
                    component:{
                        template:`
                            <div>
                                 detail
                            </div>
                        `
                    }
                }
            ]
        })

        var app = new Vue({
            el:'#app',
            router:router,
            methods:{
                change:function(){
                    setTimeout(function(){
                        this.router.push('/list')
                        setTimeout(function(){
                            this.router.replace('/detail')
                        },2000)
                    },2000)

                }
            }
        })
    </script>
</body>
</html>
```

## 2.5命名路由和视图

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>路由</title>
    <script src="https://unpkg.com/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
</head>
<body>
    <div id="app">
        <router-link :to={name:'index'}>index</router-link>

          <router-view name="info"></router-view>
    </div>


    <script>
        var router = new VueRouter({
            routes:[
                {
                    path:'/',
                    name:'index',
                    components:{
                        name:{
                            template:`
                            <div>
                                 name
                            </div>
                        `
                        },
                    info:{
                            template:`
                            <div>
                                 info
                            </div>
                        `
                        }
                    }
                }
            ]
        })

        var app = new Vue({
            el:'#app',
            router:router,
            methods:{
                change:function(){
                    setTimeout(function(){
                        this.router.push('/list')
                        setTimeout(function(){
                            this.router.replace('/detail')
                        },2000)
                    },2000)

                }
            }
        })
    </script>
</body>
</html>
```

## 2.6重定向和别名

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>路由</title>
    <script src="https://unpkg.com/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
</head>
<body>
    <div id="app">
        <router-link :to={name:'index'}>index</router-link>
        <router-link to="/list">list</router-link>
        <router-link to="/compute">detail</router-link>

          <router-view></router-view>
    </div>


    <script>
        var router = new VueRouter({
            routes:[
                {
                    path:'/',
                    redirect:'/list',
                    component:{
                        template:`
                            <div>
                                 index
                            </div>
                        `
                    }
                },
                {
                    path:'/list',
                    component:{
                        template:`
                            <div>
                                 list
                            </div>
                        `
                    }
                },
                {
                    path:'/detail',
                    component:{
                        template:`
                            <div>
                                 detail
                            </div>
                        `
                    },
                    alias:'/compute'
                }

            ]
        })

        var app = new Vue({
            el:'#app',
            router:router,
            methods:{
                change:function(){
                    setTimeout(function(){
                        this.router.push('/list')
                        setTimeout(function(){
                            this.router.replace('/detail')
                        },2000)
                    },2000)

                }
            }
        })
    </script>
</body>
</html>
```

## 2.7组件传值

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>路由</title>
    <script src="https://unpkg.com/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
</head>
<body>
    <div id="app">
        <router-link to="/foo/one">Go to Foo one</router-link>
        <router-link to="/foo/two">Go to Foo two</router-link>
        <router-link to="/s/one">Go to Foo one</router-link>
        <router-link to="/s/two">Go to Foo two</router-link>

          <router-view></router-view>
    </div>


    <script>
        var pageId = {
            template:`<div>{{name}}</div>`,
            props:['name']

        }
        var router = new VueRouter({
            routes:[
                {
                    path:'/foo/:name',
                    component:pageId,
                    props:true
                },
                {
                    path:'/s/:name',
                    component:pageId,
                    props:true
                },
                {
                    path:'/',
                    component:{
                        template:`<div>index</div>`
                    }
                }
            ]
        })

        var app = new Vue({
            el:'#app',
            router:router
        })
    </script>
</body>
</html>
```

## 2.8导航守卫

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>路由</title>
    <script src="https://unpkg.com/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
</head>
<body>
    <div id="app">
        <router-link to="/">index</router-link>
        <router-link to="/user">user</router-link>
        <router-link to="/login">login</router-link>
          <router-view></router-view>
    </div>


    <script>
        var router = new VueRouter({
            routes:[
                {
                    path:'/',
                    component:{
                        template:`<div>index</div>`
                    }
                },
                {
                    path:'/user',
                    component:{
                        template:`<div>user</div>`
                    }
                },
                {
                    path:'/login',
                    component:{
                        template:`<div>login</div>`
                    }
                }
            ]
        })
        router.beforeEach(function(to,from,next){
            var login = false;
            if(!login && to.path === '/user'){
                next('/login')
            }else{
                next()
            }
        })
        var app = new Vue({
            el:'#app',
            router:router
        })
    </script>
</body>
</html>
```

## 2.9路由元信息

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>路由</title>
    <script src="https://unpkg.com/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
</head>
<body>
    <div id="app">
        <router-link to="/">index</router-link>
        <router-link to="/user">user</router-link>
        <router-link to="/login">login</router-link>
          <router-view></router-view>
    </div>


    <script>
        var router = new VueRouter({
            routes:[
                {
                    path:'/',
                    component:{
                        template:`<div>index</div>`
                    }
                },
                {
                    path:'/user',
                    component:{
                        template:`<div>user</div>`
                    },
                    meta:{
                        flag:true
                    }
                },
                {
                    path:'/login',
                    component:{
                        template:`<div>login</div>`
                    }
                }
            ]
        })
        router.beforeEach(function(to,from,next){
            var login = true;
            if(!login && to.matched.some(function(item) {
                return item.meta.flag
            })){
                next('/login')
            }else{
                next()
            }
        })
        var app = new Vue({
            el:'#app',
            router:router
        })
    </script>
</body>
</html>
```

## 2.10过渡动效

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>路由</title>
    <link href="https://cdn.bootcss.com/animate.css/3.7.0/animate.css" rel="stylesheet">
    <script src="https://unpkg.com/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
</head>
<body>
    <div id="app">
        <router-link to="/">index</router-link>
        <router-link to="/list">list</router-link>
        <router-link to="/detail">detail</router-link>
        <transition :enter-active-class="effect">
              <router-view></router-view>
          </transition>
    </div>


    <script>
        var router = new VueRouter({
            routes:[
                {
                    path:'/',
                    component:{
                        template:`
                            <div>
                                 index
                            </div>
                        `
                    }
                },
                {
                    path:'/list',
                    component:{
                        template:`
                            <div>
                                 list
                            </div>
                        `
                    }
                },
                {
                    path:'/detail',
                    component:{
                        template:`
                            <div>
                                 detail
                            </div>
                        `
                    },
                }

            ]
        })

        var app = new Vue({
            el:'#app',
            router:router,
            data:{
                effect:''
            },
            watch:{
                '$route'(to,from){
                    var effectMap = {
                        "/":"shake",
                        "/list":"jello",
                        "/detail":"bounce"
                    };
                this.effect = ['animated',effectMap[to.path]].join(' ')
                }
            }
        })
    </script>
</body>
</html>
```

# 3.vuex基础知识

## 3.1state

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>状态管理</title>
    <script src="https://unpkg.com/vue/dist/vue.js"></script>
    <script src="https://cdn.bootcss.com/vuex/3.1.0/vuex.js"></script>
</head>
<body>
    <div id="app">
        {{count}}
    </div>


    <script>
        var store = new Vuex.Store({
            state:{
                count:0
            },
            mutations:{
                increment(state){
                    state.count++
                }
            }
        })
        store.commit('increment')

        var app = new Vue({
            el:"#app",
            store,
            computed:Vuex.mapState(['count'])
        })
    </script>
</body>
</html>
```

## 3.2getter

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>状态管理</title>
    <script src="https://unpkg.com/vue/dist/vue.js"></script>
    <script src="https://cdn.bootcss.com/vuex/3.1.0/vuex.js"></script>
</head>
<body>
    <div id="app">
        {{add(2)}}
    </div>


    <script>
        var store = new Vuex.Store({
            state:{
                count:0
            },
            getters:{
                add:state => n => {
                    return state.count + n;
                }
            }
        })

        var app = new Vue({
            el:"#app",
            store,
            computed:Vuex.mapGetters(['add'])
        })
    </script>
</body>
</html>
```

## 3.3mutations

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>状态管理</title>
    <script src="https://unpkg.com/vue/dist/vue.js"></script>
    <script src="https://cdn.bootcss.com/vuex/3.1.0/vuex.js"></script>
</head>
<body>
    <div id="app">
    </div>


    <script>
        var ADD = 'ADD'; 
        var store = new Vuex.Store({
            state:{
                count:0
            },
            mutations:{
                [ADD](state,payload){
                    state.count+=payload.n
                }
            }
        })
        store.commit({
            type:'ADD',
            n:10
        })
        console.log(store.state.count)
    </script>
</body>
</html>
```

## 3.4actions

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>状态管理</title>
    <script src="https://unpkg.com/vue/dist/vue.js"></script>
    <script src="https://cdn.bootcss.com/vuex/3.1.0/vuex.js"></script>
</head>
<body>
    <div id="app">
    </div>


    <script>
        var ADD = 'ADD'; 
        var store = new Vuex.Store({
            state:{
                count:0
            },
            mutations:{
                increment(state){
                    state.count++
                }
            },
            actions:{
                [ADD](state,payload){
                    setTimeout(()=>{
                        commit('increment')
                    },3000)
                }
            }
        })
        store.dispatch('ADD')
    </script>
</body>
</html>
```

# 4.vueSSR基础知识

## 4.1纯浏览器端渲染

`实现纯浏览器端渲染的关键是render: h => h(App)，render会将app转化成html的节点插入到页面`
`目录结构`

```
- node_modules
- components  
    - Bar.vue
    - Foo.vue
- App.vue
- app.js
- index.html
- webpack.config.js
- package.json
- yarn.lock
- postcss.config.js
- .babelrc
- .gitignore
```

`app.js`

```
import Vue from 'vue';
import App from './App.vue';

let app = new Vue({
  el: '#app',
  render: h => h(App)
});
```

`app.vue`

```
<template>
  <div>
    <Foo></Foo>
    <Bar></Bar>
  </div>
</template>

<script>
import Foo from './components/Foo.vue';
import Bar from './components/Bar.vue';

export default {
  components: {
    Foo, Bar
  }
}
</script>
```

`index.html`

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>纯浏览器渲染</title>
</head>
<body>
  <div id="app"></div>
</body>
</html>
```

`components/Foo.vue`

```
<template>
  <div class="foo">
    <h1>Foo Component</h1>
  </div>
</template>

<style>
.foo {
  background: yellowgreen;
}
</style>
```

`components/Bar.vue`

```
<template>
  <div class="bar">
    <h1>Bar Component</h1>
  </div>
</template>

<style>
.bar {
  background: bisque;
}
</style>
```

`webpack.config.js`

```
const path = require('path');
const VueLoaderPlugin = require('vue-loader/lib/plugin');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const ExtractTextPlugin = require('extract-text-webpack-plugin');

module.exports = {
  mode: 'development',

  entry: './app.js',

  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },

  module: {
    rules: [
      {
        test: /\.js$/,
        use: 'babel-loader'
      },
      {
        test: /\.css$/,
        use: ['vue-style-loader', 'css-loader', 'postcss-loader']
        // 如果需要单独抽出CSS文件，用下面这个配置
        // use: ExtractTextPlugin.extract({
        //   fallback: 'vue-style-loader',
        //   use: [
        //     'css-loader',
        //     'postcss-loader'
        //   ]
        // })
      },
      {
        test: /\.(jpg|jpeg|png|gif|svg)$/,
        use: {
          loader: 'url-loader',
          options: {
            limit: 10000    // 10Kb
          }
        }
      },
      {
        test: /\.vue$/,
        use: 'vue-loader'
      }
    ]
  },

  plugins: [
    new VueLoaderPlugin(),
    new HtmlWebpackPlugin({
      template: './index.html'
    }),
    // 如果需要单独抽出CSS文件，用下面这个配置
    // new ExtractTextPlugin("styles.css")
  ]
};
```

`postcss.config.js`

```
module.exports = {
  plugins: [
    require('autoprefixer')
  ]
};
```

`.babelrc`

```
{
  "presets": [
    "@babel/preset-env"
  ],
  "plugins": [
    // 让其支持动态路由的写法 const Foo = () => import('../components/Foo.vue')
    "dynamic-import-webpack"    
  ]
}
```

`package.json`

```
{
  "name": "01",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT",
  "scripts": {
    "start": "yarn run dev",
    "dev": "webpack-dev-server",
    "build": "webpack"
  },
  "dependencies": {
    "vue": "^2.5.17"
  },
  "devDependencies": {
    "@babel/core": "^7.1.2",
    "@babel/preset-env": "^7.1.0",
    "babel-plugin-dynamic-import-webpack": "^1.1.0",
    "autoprefixer": "^9.1.5",
    "babel-loader": "^8.0.4",
    "css-loader": "^1.0.0",
    "extract-text-webpack-plugin": "^4.0.0-beta.0",
    "file-loader": "^2.0.0",
    "html-webpack-plugin": "^3.2.0",
    "postcss": "^7.0.5",
    "postcss-loader": "^3.0.0",
    "url-loader": "^1.1.1",
    "vue-loader": "^15.4.2",
    "vue-style-loader": "^4.1.2",
    "vue-template-compiler": "^2.5.17",
    "webpack": "^4.20.2",
    "webpack-cli": "^3.1.2",
    "webpack-dev-server": "^3.1.9"
  }
}
```

`启动`

```
yarn start
```

`构建`

```
yarn run build
```

`效果图：`
![browserrender](https://segmentfault.com/img/remote/1460000018556186?w=1595&h=676)

## 4.2纯服务器端端渲染(无ajax)

`让一份代码既可以在服务端运行，也可以在客户端运行,如果说在SSR的过程中出现问题，还可以回滚到纯浏览器渲染，保证用户正常看到页面,顺着这个思路，肯定就会有两个webpack的入口文件，一个用于浏览器端渲染weboack.client.config.js，一个用于服务端渲染webpack.server.config.js，将它们的公有部分抽出来作为webpack.base.cofig.js，后续通过webpack-merge进行合并。同时，也要有一个server来提供http服务，我这里用的是koa`
`实现这些的关键是require('vue-server-renderer').createBundleRenderer(server.bundle.js,{ template:"index.ssr.html"}).renderToString((err, html) => { })`
`目录结构`

```
- node_modules
- config    // 新增
    - webpack.base.config.js
    - webpack.client.config.js
    - webpack.server.config.js
- src
    - components  
        - Bar.vue
        - Foo.vue
    - App.vue
    - app.js
    - entry-client.js   // 新增
    - entry-server.js   // 新增
    - index.html
    - index.ssr.html    // 新增
- package.json
- yarn.lock
- postcss.config.js
- .babelrc
- .gitignore
```

`对app.js做修改，将其包装为一个工厂函数，每次调用都会生成一个全新的根组件`
`app.js`

```
import Vue from 'vue';
import App from './App.vue';

export function createApp() {
  const app = new Vue({
    render: h => h(App)
  });

  return { app };
}
```

`在浏览器端，我们直接新建一个根组件，然后将其挂载就可以了`
`entry-client.js`

```
import { createApp } from './app.js';

const { app } = createApp();

app.$mount('#app');
```

`在服务器端，我们就要返回一个函数，该函数的作用是接收一个context参数，同时每次都返回一个新的根组件。这个context在这里我们还不会用到，后续的步骤会用到它`
`entry-server.js`

```
import { createApp } from './app.js';

export default context => {
  const { app } = createApp();

  return app;
}
```

`然后再来看一下index.ssr.html`

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>服务端渲染</title>
</head>
<body>
  <!--vue-ssr-outlet-->

  <script type="text/javascript" src="<%= htmlWebpackPlugin.options.files.js %>"></script>
</body>
</html>
```

`<!--vue-ssr-outlet-->的作用是作为一个占位符，后续通过vue-server-renderer插件，将服务器解析出的组件html字符串插入到这里。<script type="text/javascript" src="<%= htmlWebpackPlugin.options.files.js %>"></script>是为了将webpack通过webpack.client.config.js打包出的文件放到这里`
`因为服务端吐出来的就是一个html字符串，后续的Vue相关的响应式、事件响应等等，都需要浏览器端来接管，所以就需要将为浏览器端渲染打包的文件在这里引入,用官方的词来说，叫客户端激活（client-side hydration）`
`所谓客户端激活，指的是 Vue 在浏览器端接管由服务端发送的静态 HTML，使其变为由 Vue 管理的动态 DOM 的过程`
`在 entry-client.js 中，我们用下面这行挂载(mount)应用程序`

```
// 这里假定 App.vue template 根元素的 `id="app"`
app.$mount('#app')
```

`由于服务器已经渲染好了 HTML，我们显然无需将其丢弃再重新创建所有的 DOM 元素。相反，我们需要"激活"这些静态的 HTML，然后使他们成为动态的（能够响应后续的数据变化）。`
`如果你检查服务器渲染的输出结果，你会注意到应用程序的根元素上添加了一个特殊的属性：`

```
<div id="app" data-server-rendered="true">
```

`Vue在浏览器端就依靠这个属性将服务器吐出来的html进行激活，我们一会自己构建一下就可以看到了。接下来我们看一下webpack相关的配置：`
`webpack.base.config.js`

```
const path = require('path');
const VueLoaderPlugin = require('vue-loader/lib/plugin');

module.exports = {
  mode: 'development',

  resolve: {
    extensions: ['.js', '.vue']
  },

  output: {
    path: path.resolve(__dirname, '../dist'),
    filename: '[name].bundle.js'
  },

  module: {
    rules: [
      {
        test: /\.vue$/,
        use: 'vue-loader'
      },
      {
        test: /\.js$/,
        use: 'babel-loader'
      },
      {
        test: /\.css$/,
        use: ['vue-style-loader', 'css-loader', 'postcss-loader']
      },
      {
        test: /\.(jpg|jpeg|png|gif|svg)$/,
        use: {
          loader: 'url-loader',
          options: {
            limit: 10000    // 10Kb
          }
        }
      }
    ]
  },

  plugins: [
    new VueLoaderPlugin()
  ]
};
```

`webpack.client.config.js`

```
const path = require('path');
const merge = require('webpack-merge');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const base = require('./webpack.base.config');

module.exports = merge(base, {
  entry: {
    client: path.resolve(__dirname, '../src/entry-client.js')
  },

  plugins: [
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, '../src/index.html'),
      filename: 'index.html'
    })
  ]
});
```

`注意，这里的入口文件变成了entry-client.js，将其打包出的client.bundle.js插入到index.html中`
`webpack.server.config.js`

```
const path = require('path');
const merge = require('webpack-merge');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const base = require('./webpack.base.config');

module.exports = merge(base, {
  target: 'node',
  entry: {
    server: path.resolve(__dirname, '../src/entry-server.js')
  },
  output: {
    libraryTarget: 'commonjs2'
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, '../src/index.ssr.html'),
      filename: 'index.ssr.html',
      files: {
        js: 'client.bundle.js'
      },
      excludeChunks: ['server']
    })
  ]
});
```

`这里有几个点需要注意一下：`

- 入口文件是 entry-server.js
- 因为是打包服务器端依赖的代码，所以target要设为node，同时，output的libraryTarget要设为commonjs2

`入口文件是 entry-server.js,因为是打包服务器端依赖的代码，所以target要设为node，同时，output的libraryTarget要设为commonjs2`
`这里关于HtmlWebpackPlugin配置的意思是，不要在index.ssr.html中引入打包出的server.bundle.js，要引为浏览器打包的client.bundle.js，原因前面说过了，是为了让Vue可以将服务器吐出来的html进行激活，从而接管后续响应。`
`那么打包出的server.bundle.js在哪用呢？接着往下看就知道啦`

`package.json`

```
{
  "name": "01",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT",
  "scripts": {
    "start": "yarn run dev",
    "dev": "webpack-dev-server",
    "build:client": "webpack --config config/webpack.client.config.js",
    "build:server": "webpack --config config/webpack.server.config.js"
  },
  "dependencies": {
    "koa": "^2.5.3",
    "koa-router": "^7.4.0",
    "koa-static": "^5.0.0",
    "vue": "^2.5.17",
    "vue-server-renderer": "^2.5.17"
  },
  "devDependencies": {
    "@babel/core": "^7.1.2",
    "@babel/preset-env": "^7.1.0",
    "autoprefixer": "^9.1.5",
    "babel-loader": "^8.0.4",
    "css-loader": "^1.0.0",
    "extract-text-webpack-plugin": "^4.0.0-beta.0",
    "file-loader": "^2.0.0",
    "html-webpack-plugin": "^3.2.0",
    "postcss": "^7.0.5",
    "postcss-loader": "^3.0.0",
    "style-loader": "^0.23.0",
    "url-loader": "^1.1.1",
    "vue-loader": "^15.4.2",
    "vue-style-loader": "^4.1.2",
    "vue-template-compiler": "^2.5.17",
    "webpack": "^4.20.2",
    "webpack-cli": "^3.1.2",
    "webpack-dev-server": "^3.1.9",
    "webpack-merge": "^4.1.4"
  }
}
```

`接下来我们看server端关于http服务的代码:`
`server/server.js`

```
const Koa = require('koa');
const Router = require('koa-router');
const serve = require('koa-static');
const path = require('path');
const fs = require('fs');
const backendApp = new Koa();
const frontendApp = new Koa();
const backendRouter = new Router();
const frontendRouter = new Router();

const bundle = fs.readFileSync(path.resolve(__dirname, '../dist/server.bundle.js'), 'utf-8');
const renderer = require('vue-server-renderer').createBundleRenderer(bundle, {
  template: fs.readFileSync(path.resolve(__dirname, '../dist/index.ssr.html'), 'utf-8')
});

// 后端Server
backendRouter.get('/index', (ctx, next) => {
  // 这里用 renderToString 的 promise 返回的 html 有问题，没有样式
  renderer.renderToString((err, html) => {
    if (err) {
      console.error(err);
      ctx.status = 500;
      ctx.body = '服务器内部错误';
    } else {
      console.log(html);
      ctx.status = 200;
      ctx.body = html;
    }
  });
});

backendApp.use(serve(path.resolve(__dirname, '../dist')));

backendApp
  .use(backendRouter.routes())
  .use(backendRouter.allowedMethods());

backendApp.listen(3000, () => {
  console.log('服务器端渲染地址： http://localhost:3000');
});


// 前端Server
frontendRouter.get('/index', (ctx, next) => {
  let html = fs.readFileSync(path.resolve(__dirname, '../dist/index.html'), 'utf-8');
  ctx.type = 'html';
  ctx.status = 200;
  ctx.body = html;
});

frontendApp.use(serve(path.resolve(__dirname, '../dist')));

frontendApp
  .use(frontendRouter.routes())
  .use(frontendRouter.allowedMethods());

frontendApp.listen(3001, () => {
  console.log('浏览器端渲染地址： http://localhost:3001');
});
```

`这里对两个端口进行监听，3000端口是服务端渲染，3001端口是直接输出index.html，然后会在浏览器端走Vue的那一套，主要是为了和服务端渲染做对比使用。这里的关键代码是如何在服务端去输出html字符串`

```
const bundle = fs.readFileSync(path.resolve(__dirname, '../dist/server.bundle.js'), 'utf-8');
const renderer = require('vue-server-renderer').createBundleRenderer(bundle, {
  template: fs.readFileSync(path.resolve(__dirname, '../dist/index.ssr.html'), 'utf-8')
});
```

`可以看到，server.bundle.js在这里被使用啦，因为它的入口是一个函数，接收context作为参数（非必传），输出一个根组件app`
`这里我们用到了vue-server-renderer插件，它有两个方法可以做渲染，一个是createRenderer，另一个是createBundleRenderer`

```
const { createRenderer } = require('vue-server-renderer')
const renderer = createRenderer({ /* 选项 */ })
```

```
const { createBundleRenderer } = require('vue-server-renderer')
const renderer = createBundleRenderer(serverBundle, { /* 选项 */ })
```

`createRenderer无法接收为服务端打包出的server.bundle.js文件，所以这里只能用createBundleRenderer`
`serverBundle 参数可以是以下之一：`

- 绝对路径，指向一个已经构建好的 bundle 文件（.js 或 .json）。必须以 / 开头才会被识别为文件路径
- 由 webpack + vue-server-renderer/server-plugin 生成的 bundle 对象。
- JavaScript 代码字符串（不推荐）。

`这里我们引入的是.js文件，后续会介绍如何使用.json文件以及有什么好处。`

```
renderer.renderToString((err, html) => {
    if (err) {
      console.error(err);
      ctx.status = 500;
      ctx.body = '服务器内部错误';
    } else {
      console.log(html);
      ctx.status = 200;
      ctx.body = html;
    }
});
```

`使用createRenderer和createBundleRenderer返回的renderer函数包含两个方法renderToString和renderToStream，我们这里用的是renderToString成功后直接返回一个完整的字符串，renderToStream返回的是一个Node流。`
`renderToString支持Promise，但是我在使用Prmoise形式的时候样式会渲染不出来，暂时还不知道原因`
`配置基本就完成了，来看一下如何运行。`

```
yarn run build:client       // 打包浏览器端需要bundle
yarn run build:server       // 打包SSR需要bundle

yarn start      // 其实就是 node server/server.js，提供http服务
```

![serverrender](https://segmentfault.com/img/remote/1460000018556187)

## 4.3纯服务器端端渲染(有ajax)

`如果SSR需要初始化一些异步数据，那么流程就会变得复杂一些,我们先提出几个问题：`

- 服务端拿异步数据的步骤在哪做？
- 如何确定哪些组件需要获取异步数据？
- 获取到异步数据之后要如何塞回到组件内？

`服务器端渲染和浏览器端渲染组件经过的生命周期是有区别的，在服务器端，只会经历beforeCreate和created两个生命周期。因为SSR服务器直接吐出html字符串就好了，不会渲染DOM结构，所以不存在beforeMount和mounted的，也不会对其进行更新，所以也就不存在beforeUpdate和updated等`
`我们先来想一下，在纯浏览器渲染的Vue项目中，我们是怎么获取异步数据并渲染到组件中的？一般是在created或者mounted生命周期里发起异步请求，然后在成功回调里执行this.data = xxx，Vue监听到数据发生改变，走后面的Dom Diff，打patch，做DOM更新`
`那么服务端渲染可不可以也这么做呢？答案是不行的。`

- 在mounted里肯定不行，因为SSR都没有mounted生命周期，所以在这里肯定不行。
- 在beforeCreate里发起异步请求是否可以呢，也是不行的。因为请求是异步的，可能还没有等接口返回，服务端就已经把html字符串拼接出来了。

`那应该怎么做呢？`

- 在渲染前，要预先获取所有需要的异步数据，然后存到Vuex的store中。
- 在后端渲染时，通过Vuex将获取到的数据注入到相应组件中。
- 把store中的数据设置到window.__INITIAL_STATE__属性中。
- 在浏览器环境中，通过Vuex将window.__INITIAL_STATE__里面的数据注入到相应组件中。

`正常情况下，通过这几个步骤，服务端吐出来的html字符串相应组件的数据都是最新的，所以第4步并不会引起DOM更新，但如果出了某些问题，吐出来的html字符串没有相应数据，Vue也可以在浏览器端通过`Vuex注入数据，进行DOM更新。`
`更新后的目录`

```
- node_modules
- config
   - webpack.base.config.js
   - webpack.client.config.js
   - webpack.server.config.js
- src
   - components  
       - Bar.vue
       - Foo.vue
   - store             // 新增
       store.js
   - App.vue
   - app.js
   - entry-client.js
   - entry-server.js   
   - index.html
   - index.ssr.html
- package.json
- yarn.lock
- postcss.config.js
- .babelrc
- .gitignore
```

`store/store.js`

```
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

const fetchBar = function() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('bar 组件返回 ajax 数据');
    }, 1000);
  });
};

function createStore() {
  const store = new Vuex.Store({
    state: {
      bar: ''
    },

    mutations: {
      'SET_BAR'(state, data) {
        state.bar = data;
      }
    },

    actions: {
      fetchBar({ commit }) {
        return fetchBar().then((data) => {
          commit('SET_BAR', data);
        }).catch((err) => {
          console.error(err);
        })
      }
    }
  });

  if (typeof window !== 'undefined' && window.__INITIAL_STATE__) {
    console.log('window.__INITIAL_STATE__', window.__INITIAL_STATE__);
    store.replaceState(window.__INITIAL_STATE__);
  }
  
  return store;
}

export default createStore;

typeof window
```

`这里fetchBar可以看成是一个异步请求，这里用setTimeout模拟。在成功回调中commit相应的mutation进行状态修改。`

```
if (typeof window !== 'undefined' && window.__INITIAL_STATE__) {
    console.log('window.__INITIAL_STATE__', window.__INITIAL_STATE__);
    store.replaceState(window.__INITIAL_STATE__);
}
```

`因为store.js同样也会被打包到服务器运行的server.bundle.js中，所以运行环境不一定是浏览器，这里需要对window做判断，防止报错，同时如果有window.__INITIAL_STATE__属性，说明服务器已经把所有初始化需要的异步数据都获取完成了，要对store中的状态做一个替换，保证统一`
`components/Bar.vue`

```
<template>
  <div class="bar">
    <h1 @click="onHandleClick">Bar Component</h1>
    <h2>异步Ajax数据：</h2>
    <span>{{ msg }}</span>
  </div>
</template>

<script>
  const fetchInitialData = ({ store }) => {
    store.dispatch('fetchBar');
  };

  export default {
    asyncData: fetchInitialData,

    methods: {
      onHandleClick() {
        alert('bar');
      }
    },

    mounted() {
      // 因为服务端渲染只有 beforeCreate 和 created 两个生命周期，不会走这里
      // 所以把调用 Ajax 初始化数据也写在这里，是为了供单独浏览器渲染使用
      let store = this.$store;
      fetchInitialData({ store });
    },

    computed: {
      msg() {
        return this.$store.state.bar;
      }
    }
  }
</script>

<style>
.bar {
  background: bisque;
}
</style>
```

`这里在Bar组件的默认导出对象中增加了一个方法asyncData，在该方法中会dispatch相应的action，进行异步数据获取。需要注意的是，我在mounted中也写了获取数据的代码，这是为什么呢？ 因为想要做到同构，代码单独在浏览器端运行，也应该是没有问题的，又由于服务器没有mounted生命周期，所以我写在这里就可以解决单独在浏览器环境使用也可以发起同样的异步请求去初始化数据。`

`components/Foo.vue`

```
<template>
  <div class="foo">
    <h1 @click="onHandleClick">Foo Component</h1>
  </div>
</template>

<script>
export default {
  methods: {
    onHandleClick() {
      alert('foo');
    }
  },
}
</script>

<style>
.foo {
  background: yellowgreen;
}
</style>
```

`这里我对两个组件都添加了一个点击事件，为的是证明在服务器吐出首页html后，后续的步骤都会被浏览器端的Vue接管，可以正常执行后面的操作`
`app.js`

```
import Vue from 'vue';
import createStore from './store/store.js';
import App from './App.vue';

export function createApp() {
  const store = createStore();

  const app = new Vue({
    store,
    render: h => h(App)
  });

  return { app, store, App };
}
```

`在建立根组件的时候，要把Vuex的store传进去，同时要返回，后续会用到。最后来看一下entry-server.js，关键步骤在这里：`
`entry-server.js`

```
import { createApp } from './app.js';

export default context => {
  return new Promise((resolve, reject) => {
    const { app, store, App } = createApp();

    let components = App.components;
    let asyncDataPromiseFns = [];
  
    Object.values(components).forEach(component => {
      if (component.asyncData) {
        asyncDataPromiseFns.push(component.asyncData({ store }));
      }
    });
  
    Promise.all(asyncDataPromiseFns).then((result) => {
      // 当使用 template 时，context.state 将作为 window.__INITIAL_STATE__ 状态，自动嵌入到最终的 HTML 中
      context.state = store.state;
  
      console.log(222);
      console.log(store.state);
      console.log(context.state);
      console.log(context);
  
      resolve(app);
    }, reject);
  });
}
```

`我们通过导出的App拿到了所有它下面的components，然后遍历，找出哪些component有asyncData方法，有的话调用并传入store，该方法会返回一个Promise，我们使用Promise.all等所有的异步方法都成功返回，才resolve(app)。context.state = store.state作用是，当使用createBundleRenderer时，如果设置了template选项，那么会把context.state的值作为window.__INITIAL_STATE__自动插入到模板html中。`
`运行：`

```
yarn run build:client
yarn run build:server

yarn start
```

![sdf](https://segmentfault.com/img/remote/1460000018556188)

`可以看到window.__INITIAL_STATE__被自动插入了。我们来对比一下SSR到底对加载性能有什么影响吧。服务端渲染时performance截图：`
![fghj](https://segmentfault.com/img/remote/1460000018556189)
`纯浏览器端渲染时performance截图：`
![ffsdvs](https://segmentfault.com/img/remote/1460000018556190)

`同样都是在fast 3G网络模式下，纯浏览器端渲染首屏加载花费时间2.9s，因为client.js加载就花费了2.27s，因为没有client.js就没有Vue，也就没有后面的东西了。服务端渲染首屏时间花费0.8s，虽然client.js加载扔花费2.27s，但是首屏已经不需要它了，它是为了让Vue在浏览器端进行后续接管。从这我们可以真正的看到，服务端渲染对于提升首屏的响应速度是很有作用的。当然有的同学可能会问，在服务端渲染获取初始ajax数据时，我们还延时了1s，在这个时间用户也是看不到页面的。没错，接口的时间我们无法避免，就算是纯浏览器渲染，首页该调接口还是得调，如果接口响应慢，那么纯浏览器渲染看到完整页面的时间会更慢。`

## 4.4serverBundle和clientManifest进行优化

`前面我们创建服务端renderer的方法是：`

```
const bundle = fs.readFileSync(path.resolve(__dirname, '../dist/server.js'), 'utf-8');
const renderer = require('vue-server-renderer').createBundleRenderer(bundle, {
  template: fs.readFileSync(path.resolve(__dirname, '../dist/index.ssr.html'), 'utf-8')
});
```

`serverBundle我们用的是打包出的server.bundle.js文件。这样做的话，在每次编辑过应用程序源代码之后，都必须停止并重启服务。这在开发过程中会影响开发效率。此外，Node.js 本身不支持 source map`
`vue-server-renderer 提供一个名为 createBundleRenderer 的 API，用于处理此问题，通过使用 webpack 的自定义插件，server bundle 将生成为可传递到 bundle renderer 的特殊 JSON 文件。所创建的 bundle renderer，用法和普通 renderer 相同，但是 bundle renderer 提供以下优点:`

- 内置的 source map 支持（在 webpack 配置中使用 devtool: 'source-map'）
- 在开发环境甚至部署过程中热重载（通过读取更新后的 bundle，然后重新创建 renderer 实例）
- 关键 CSS(critical CSS) 注入（在使用 *.vue 文件时）：自动内联在渲染过程中用到的组件所需的CSS
- 使用 clientManifest 进行资源注入：自动推断出最佳的预加载(preload)和预取(prefetch)指令，以及初始渲染所需的代码分割 chunk。

`那么我们来修改webpack配置：`
`webpack.client.config.js`

```
const path = require('path');
const merge = require('webpack-merge');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const VueSSRClientPlugin = require('vue-server-renderer/client-plugin');
const base = require('./webpack.base.config');

module.exports = merge(base, {
  entry: {
    client: path.resolve(__dirname, '../src/entry-client.js')
  },

  plugins: [
    new VueSSRClientPlugin(),   // 新增
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, '../src/index.html'),
      filename: 'index.html'
    })
  ]
});
```

`webpack.server.config.js`

```
const path = require('path');
const merge = require('webpack-merge');
const nodeExternals = require('webpack-node-externals');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const VueSSRServerPlugin = require('vue-server-renderer/server-plugin');
const base = require('./webpack.base.config');

module.exports = merge(base, {
  target: 'node',
   // 对 bundle renderer 提供 source map 支持
  devtool: '#source-map',
  entry: {
    server: path.resolve(__dirname, '../src/entry-server.js')
  },
  externals: [nodeExternals()],     // 新增
  output: {
    libraryTarget: 'commonjs2'
  },
  plugins: [
    new VueSSRServerPlugin(),   // 这个要放到第一个写，否则 CopyWebpackPlugin 不起作用，原因还没查清楚
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, '../src/index.ssr.html'),
      filename: 'index.ssr.html',
      files: {
        js: 'client.bundle.js'
      },
      excludeChunks: ['server']
    })
  ]
});
```

`因为是服务端引用模块，所以不需要打包node_modules中的依赖，直接在代码中require引用就好，所以配置externals: [nodeExternals()]`
`两个配置文件会分别生成vue-ssr-client-manifest.json和vue-ssr-server-bundle.json。作为createBundleRenderer的参数`
`来看server.js:`

```
const serverBundle = require(path.resolve(__dirname, '../dist/vue-ssr-server-bundle.json'));
const clientManifest = require(path.resolve(__dirname, '../dist/vue-ssr-client-manifest.json'));
const template = fs.readFileSync(path.resolve(__dirname, '../dist/index.ssr.html'), 'utf-8');

const renderer = createBundleRenderer(serverBundle, {
  runInNewContext: false,
  template: template,
  clientManifest: clientManifest
});
```

`效果和第三步就是一样的啦`

## 4.5基于Vue + VueRouter + Vuex的SSR

`在src下新增router目录:`

```
import Vue from 'vue';
import Router from 'vue-router';
import Bar from '../components/Bar.vue';

Vue.use(Router);

function createRouter() {
  const routes = [
    {
      path: '/bar',
      component: Bar
    },
    {
      path: '/foo',
      component: () => import('../components/Foo.vue')   // 异步路由
    }
  ];

  const router = new Router({
    mode: 'history',
    routes
  });

  return router;
}

export default createRouter;
```

`这里我们把Foo组件作为一个异步组件引入，做成按需加载。在app.js中引入router，并导出：`
`app.js`

```
import Vue from 'vue';
import createStore from './store/store.js';
import createRouter from './router';
import App from './App.vue';

export function createApp() {
  const store = createStore();
  const router = createRouter();

  const app = new Vue({
    router,
    store,
    render: h => h(App)
  });

  return { app, store, router, App };
}
```

`修改App.vue引入路由组件：`
`App.vue`

```
<template>
  <div id="app">
    <router-link to="/bar">Goto Bar</router-link> 
    <router-link to="/foo">Goto Foo</router-link> 
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  beforeCreate() {
    console.log('App.vue beforeCreate');
  },

  created() {
    console.log('App.vue created');
  },

  beforeMount() {
    console.log('App.vue beforeMount');
  },

  mounted() {
    console.log('App.vue mounted');
  }
}
</script>
```

`最重要的修改在entry-server.js中，`

```
import { createApp } from './app.js';

export default context => {
  return new Promise((resolve, reject) => {
    const { app, store, router, App } = createApp();

    router.push(context.url);

    router.onReady(() => {
      const matchedComponents = router.getMatchedComponents();

      console.log(context.url)
      console.log(matchedComponents)

      if (!matchedComponents.length) {
        return reject({ code: 404 });
      }

      Promise.all(matchedComponents.map(component => {
        if (component.asyncData) {
          return component.asyncData({ store });
        }
      })).then(() => {
        // 当使用 template 时，context.state 将作为 window.__INITIAL_STATE__ 状态，自动嵌入到最终的 HTML 中
        context.state = store.state;

        // 返回根组件
        resolve(app);
      });
    }, reject);
  });
}
```

`这里前面提到的context就起了大作用，它将用户访问的url地址传进来，供vue-router使用。因为有异步组件，所以在router.onReady的成功回调中，去找该url路由所匹配到的组件，获取异步数据那一套还和前面的一样。于是，我们就完成了一个基本完整的基于Vue + VueRouter + VuexSSR配置`
`访问http://localhost:3000/bar：`
![hhg](https://segmentfault.com/img/remote/1460000018556191)

参考链接：
带你五步学会Vue SSR ：[https://segmentfault.com/a/11...](https://segmentfault.com/a/1190000016637877#articleHeader3)

