## [vue-better-scroll 的使用](https://www.cnblogs.com/wangtianyifang/p/9261185.html)

## 首先安装better-scroll

```
npm i better-scroll -S
```

- 1

## goods页面模板

```
<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
        <li v-for="item in goods" class="menu-item">
          <span class="text  border-1px">
            <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}

          </span>

        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foodsWrapper">
      <ul>
        <li v-for="item in goods" >
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li v-for="food in item.foods" class="food-item  border-1px">
              <div class="icon">
                <img :src="food.icon" alt="" width="57" height="57">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span class="food-number">月售{{food.sellCount}}份</span>
                  <span>好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="nowPrice">￥{{food.price}}</span>
                  <span  class="oldPrice">￥{{food.oldPrice}}</span>
                </div>
              </div>

            </li>
          </ul>
        </li>
      </ul>

    </div>
  </div>
</template>
```

 

## js

```
<script type="text/ecmascript-6">
 /* eslint-disable*/
  
  import BScroll from 'better-scroll'
export default{

    props:{
        seller:{
           type:Object
        }
    },
  data(){
        return{
            goods:[]
        }
  },
  created(){
        this.classMap=['decrease', 'discount', 'special', 'invoice', 'guarantee']
        this.$http.get('/api/goods').then((res)=>{
            this.goods=res.data.data;
            this.$nextTick(()=>{
              this._initScroll();
            })
          console.log(this.$refs.menuWrapper)



        })

  },
  methods:{
      _initScroll(){
          this.meunScroll=new BScroll(this.$refs.menuWrapper,{});
          this.foodsScroll=new BScroll(this.$refs.foodsWrapper,{});
    }

  }
}
</script>

先用ref 绑定事件， 在vue中 用$ .refs注册
在钩子函数 create中 用vue-resource 请求数据，并异步调用方法
```

```
this.$nextTick(()=>{
              this._initScroll();
            }
```

   **注册方法**

​    

```
_initScroll(){
          this.meunScroll=new BScroll(this.$refs.menuWrapper,{});
          this.foodsScroll=new BScroll(this.$refs.foodsWrapper,{});
    }
```

## better-scroll用法

我们先来看一下 better-scroll 常见的 html 结构：

```
<div class="wrapper">
 <ul class="content"> 
      <li></li>
      <li></li> 
      <li></li>
      <li></li> 
  </ul>
</div>
```

 

当 content 的高度不超过父容器的高度，是不能滚动的，而它一旦超过了父容器的高度，我们就可以滚动内容区了，这就是 better-scroll 的滚动原理。

```
 import BScroll from 'better-scroll'
 let wrapper = document.querySelector('.wrapper')
 let scroll = new BScroll(wrapper, {})
```

better-scroll 对外暴露了一个 BScroll 的类，我们初始化只需要 new 一个类的实例即可。第一个参数就是我们 wrapper 的 DOM 对象，第二个是一些配置参数。 
better-scroll 的初始化时机很重要，因为它在初始化的时候，会计算父元素和子元素的高度和宽度，来决定是否可以纵向和横向滚动。因此，我们在初始化它的时候，必须确保父元素和子元素的内容已经正确渲染了。如果没有办法滑动，那就是初始化的时机不对。 
饿了么是这样处理的：

```
mounted() {
   this.$nextTick(() => {
    this.scroll = new Bscroll(this.$refs.wrapper, {}) }) 
   }
```

 

`this.$nextTick()`这个方法作用是当数据被修改后使用这个方法会回调获取更新后的dom再render出来 
如果不在下面的`this.$nextTick()`方法里回调这个方法，数据改变后再来计算滚动轴就会出错

### 上拉刷新功能

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 <div class="wrapper" ref="wrapper">
 2         <ul class="content" ref="content">
 3           <li v-for="(item,key) in detail" :key="key" v-if="detail.length > 0">
 4             <Row type="flex" justify="start" align="middle">
 5               <Col :span="8" class="detail-item">
 6               <span :class="{'color-red':item.is_delay === 1}">{{item.order_sn}}</span>
 7               </Col>
 8               <Col :span="8" class="detail-item">
 9               <span>{{item.date}}</span>
10               </Col>
11               <Col :span="8" class="detail-item">
12               <span>￥ {{item.partner_profit  | number2}}</span>
13               </Col>
14             </Row>
15           </li>
16           <li v-if="!scrollFinish">
17             <Row type="flex" justify="center" align="middle">
18               <Col :span="6" v-if="loadingText">
19               <p>{{loadingText}}</p>
20               </Col>
21               <Col :span="2" v-else>
22                 <Spin size="large"></Spin>
23               </Col>
24             </Row>
25           </li>
26         </ul>
27       </div>
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 mounted() {
 2       // 设置wrapper的高度
 3       this.$refs.wrapper.style.height = document.getElementById("app").offsetHeight - document.getElementById("scroll").offsetTop + "px";
 4       // better-scroll 的content高度不大于wrapper高度就不能滚动,这里的问题是,当一页数据的高度不够srapper的高度的时候,即使存在n页也不能下拉
 5       // 需要设置content的min-height大于wrapper高度
 6       this.$refs.content.style.minHeight = this.$refs.wrapper.offsetHeight + 1 + "px";
 7       this._initScroll();
 8       this.getIncomeDetail(this.nextPage);
 9       // 设置scroll的高度
10       // this.scrollHeight = document.getElementById("app").offsetHeight - document.getElementById("scroll").offsetTop ;
11     },
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
methods:{
    _initScroll() {
        this.orderScroll = new BScroll(this.$refs.wrapper, {
          probeType: 3,    
          click:true,
          pullUpLoad: {   // 配置上啦加载
            threshold: -80   //上拉80px的时候加载
          },
          mouseWheel: {    // pc端同样能滑动
            speed: 20,
            invert: false
          },
          useTransition:false,  // 防止iphone微信滑动卡顿
        });
        // 上拉加载数据
        this.orderScroll.on('pullingUp',()=>{
          this.scrollFinish = false;
          // 防止一次上拉触发两次事件,不要在ajax的请求数据完成事件中调用下面的finish方法,否则有可能一次上拉触发两次上拉事件
          this.orderScroll.finishPullUp();
          // 加载数据
          this.getIncomeDetail(this.nextPage);
        });
      }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 