# [Vue--过渡动画实现的三种方式](https://www.cnblogs.com/mrszhou/p/7859020.html)

## 一.使用vue的transition标签结合css样式完成动画

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 <!DOCTYPE html>
 2 <html lang="en">
 3 <head>
 4 <meta charset="UTF-8">
 5 <meta name="viewport" content="width=device-width, initial-scale=1.0">
 6 <meta http-equiv="X-UA-Compatible" content="ie=edge">
 7 <title>Document</title>   
 8 <style>
 9     /* .v-enter-active,.v-leave-active{
10         transition:all 2s;
11     }
12     .v-enter,.v-leave-to{
13         margin-left: 100px;
14     }
15     .v-enter-to,.v-leave{
16         margin-left:0px;
17     } */
18     .show-enter-active,.show-leave-active{
19         transition:all 2s;
20     }
21     .show-enter,.show-leave-to{
22         margin-left: 100px;
23     }
24     .show-enter-to,.show-leave{
25         margin-left:0px;
26     }
27 </style>
28 <script src="../vue2.4.4.js"></script>
29 </head>
30 
31 <body>
32 <!-- 定义一个vue的管理区块，（MVVM中的View） -->
33 <!-- 
34      如果要使用动画效果:
35         第一种方式是使用transition标签加css样式结合完成：
36         1.给需要运动的元素加入transition标签
37         2.默认情况下如果控制了transition标签元素的显示和隐藏它会默认
38         给这个元素加入一些class
39             隐藏: 加入类名:
40                     v-leave  
41                     v-leave-active 
42                     v-leave-to
43 
44             显示: 加入类名:
45                     v-enter  准备进行运动的状态(起始状态)
46                     v-enter-active  整个运动状态
47                     v-enter-to  整个运动状态(强调运动的结果，结束状态)
48         3.将来如果给transition设置一个name为“show”以后,将来所有的类的名称都需要改变，默认前缀为v-
49           如果加入了name属性，需要将v- 改为show-
50  -->
51 <div id="app">
52       <button @click="toggle">显示/隐藏</button><br>
53       <!-- <transition > -->
54       <transition name="show" >
55             <span class="show" v-show="isshow">it 传说</span>
56       </transition>
57 </div>
58 
59 </body>
60 
61 <script>
62 
63     // 实例化vue对象（MVVM中的View Model）
64     new Vue({
65         // vm控制的区块为id为app的div，此div中的所有vue指令均可以被vm解析
66         el:'#app',
67         data:{
68         // 数据 （MVVM中的Model）
69             isshow:false
70         },
71         methods:{
72             toggle:function(){
73                 this.isshow = !this.isshow;
74             }
75         }
76     })
77 </script>
78 </html>
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

## 二.利用animate.css结合transition实现动画

**animate.css   演示地址 : https://daneden.github.io/animate.css/**

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) View Code

实例代码:

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 <!DOCTYPE html>
 2 <html lang="en">
 3 <head>
 4 <meta charset="UTF-8">
 5 <meta name="viewport" content="width=device-width, initial-scale=1.0">
 6 <meta http-equiv="X-UA-Compatible" content="ie=edge">
 7 <title>Document</title>   
 8 <link rel="stylesheet" href="../animate.css">
 9 <script src="../vue2.4.4.js"></script>
10 </head>
11 
12 <body>
13 <!-- 定义一个vue的管理区块，（MVVM中的View） -->
14 <div id="app">
15     <button @click="toggle">显示/隐藏</button><br>
16     <transition 
17     enter-active-class="animated fadeInRight"
18     leave-active-class="animated fadeOutRight"
19     >
20     <!-- 坑：span行内元素（行内元素没有宽） 
21          应该改为块级元素
22     -->
23           <!-- <span class="show" v-show="isshow">it创业</span> -->
24           <div style="width:200px" class="show" v-show="isshow">it创业</div> 
25     </transition>
26 </div>
27 </body>
28 
29 <script>
30 
31     // 实例化vue对象（MVVM中的View Model）
32     new Vue({
33         // vm控制的区块为id为app的div，此div中的所有vue指令均可以被vm解析
34         el:'#app',
35         data:{
36         // 数据 （MVVM中的Model）   
37             isshow:false
38         },
39         methods:{
40             toggle:function() {
41                 this.isshow = !this.isshow;
42             }
43         }
44     })
45 </script>
46 </html>
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

## 三.利用 vue中的钩子函数实现动画

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 <!DOCTYPE html>
 2 <html lang="en">
 3 
 4 <head>
 5     <meta charset="UTF-8">
 6     <meta name="viewport" content="width=device-width, initial-scale=1.0">
 7     <meta http-equiv="X-UA-Compatible" content="ie=edge">
 8     <title>Document</title>
 9     <style>
10         .show {
11             transition: all 0.5s;
12         }
13     </style>
14     <script src="../vue2.4.4.js"></script>
15 </head>
16 
17 <body>
18     <!-- 定义一个vue的管理区块，（MVVM中的View） -->
19     <div id="app">
20         <button @click="toggle">显示/隐藏</button><br>
21         <transition @before-enter="beforeEnter" @enter="enter" @after-enter="afterEnter">
22             <div class="show"  v-show="isshow">itcast 传智</div>
23         </transition>
24     </div>
25 
26 </body>
27 
28 <script>
29     // 实例化vue对象（MVVM中的View Model）
30     new Vue({
31         // vm控制的区块为id为app的div，此div中的所有vue指令均可以被vm解析
32         el: '#app',
33         data: {
34             // 数据 （MVVM中的Model）   
35             isshow: false
36         },
37         methods: {
38             toggle: function () {
39                 this.isshow = !this.isshow;
40             },
41             // 以下三个与enter相关的方法只会在元素由隐藏变为显示的时候才会执行
42             // el:指的是当前调用这个方法的元素对象
43             // done:用来决定是否要执行后续的代码如果不执行这个方法，那么将来执行完before执行完enter以后动画就会停止  
44             beforeEnter: function (el) {
45                 console.log("beforeEnter");
46                 // 当入场之前会执行 v-enter
47                 el.style = "padding-left:100px";
48             },
49             enter: function (el, done) {
50                 // 当进行的过程中每执行 v-enter-active
51                 console.log("enter");
52                 // 为了能让代码正常进行，在设置了结束状态后必须调用一下这个元素的
53                 // offsetHeight / offsetWeight  只是为了让动画执行
54                 el.offsetHeight;
55 
56                 // 结束的状态最后啊写在enter中
57                 el.style = "padding-left:0px";
58 
59 
60                 // 执行done继续向下执行
61                 done();
62             },
63             afterEnter: function (el) {
64                 // 当执行完毕以后会执行
65                 console.log("afterEnter");
66                 this.isshow = false;
67             }
68         }
69     })
70 
71 </script>
72 
73 </html>
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

## 四.完成品牌管理案例的vue中的动画完成删除提示

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
  1 <!DOCTYPE html>
  2 <html lang="en">
  3 
  4 <head>
  5     <meta charset="UTF-8">
  6     <meta name="viewport" content="width=device-width, initial-scale=1.0">
  7     <meta http-equiv="X-UA-Compatible" content="ie=edge">
  8     <title>Document</title>
  9     <style>
 10         #app {
 11             width: 600px;
 12             margin: 10px auto;
 13         }
 14 
 15         .tb {
 16             border-collapse: collapse;
 17             width: 100%;
 18         }
 19 
 20         .tb th {
 21             background-color: #0094ff;
 22             color: white;
 23         }
 24 
 25         .tb td,
 26         .tb th {
 27             padding: 5px;
 28             border: 1px solid black;
 29             text-align: center;
 30         }
 31 
 32         .add {
 33             padding: 5px;
 34             border: 1px solid black;
 35             margin-bottom: 10px;
 36         }
 37 
 38         .del li{
 39         list-style: none;
 40         padding: 10px;
 41       }
 42 
 43     .del{
 44         position: absolute;
 45         top:45%;   
 46         left: 45%;                 
 47         width: 300px;
 48         border: 1px solid rgba(0,0,0,0.2);
 49         transition: all 0.5s;
 50     }
 51     </style>
 52     <script src="../vue2.4.4.js"></script>
 53 </head>
 54 
 55 <body>
 56     <div id="app">
 57         <div class="add">
 58             编号: <input id="id" v-color="color" v-focus type="text" v-model="id">品牌名称: <input v-model="name" type="text">
 59             <button @click="add">添加</button>
 60         </div>
 61         <div class="add">品牌名称：<input type="text"></div>
 62         <div>
 63             <table class="tb">
 64                 <tr>
 65                     <th>编号</th>
 66                     <th>品牌名称</th>
 67                     <th>创立时间</th>
 68                     <th>操作</th>
 69                 </tr>
 70                 <tr v-if="list.length <= 0">
 71                     <td colspan="4">没有品牌数据</td>
 72                 </tr>
 73                 <!--加入: key="index" 时候必须把所有参数写完整  -->
 74                 <tr v-for="(item,key,index) in list" :key="index">
 75                     <td>{{item.id}}</td>
 76                     <td>{{item.name}}</td>
 77                     <td>{{item.ctime | dateFrm("/")}}</td>
 78                     <!-- 使用vue来注册事件时，我们在dom元素中是看不到的 -->
 79                     <td><a href="javascript:void(0)" @click="del(item.id)">删除</a></td>
 80                 </tr>
 81             </table>
 82         </div>
 83 
 84         <transition
 85             @before-enter="beforeEnter" 
 86             @enter="enter"
 87             @after-enter ="afterEnter"
 88             @before-leave="beforeLeave" 
 89             @leave="leave"
 90             @after-leave ="afterLeave"
 91         >
 92             <div class="del" v-show="isshow">
 93                 <ul>
 94                     <li>您确定要删除当前数据吗</li>
 95                     <li>
 96                         <button @click="delById">确定</button>
 97                         <button @click="showClose">关闭</button>
 98                     </li>
 99                 </ul>
100             </div>
101         </transition>
102 
103     </div>
104 </body>
105 
106 </html>
107 
108 <script>
109     // 使用全局过滤器（公有过滤器）
110     Vue.filter("dateFrm", function (time,spliceStr) {
111         // return "2017-11-16";
112         var date = new Date(time);
113         //得到年
114         var year = date.getFullYear();
115         // 得到月
116         var month = date.getMonth() + 1;
117         // 得到日
118         var day = date.getDate();
119         return year + spliceStr + month + spliceStr + day;
120     });
121 
122 
123     // 先将自定义指令定义好
124     // directive有两个参数
125     //参数一： 自定义指令 v-focus
126     //参数二： 对象，对象中可以添加很多方法
127     // 添加一个inserted方法:而这个方法中又有两个参数：
128     //参数一:el 当前使用自定义指令的对象
129     //参数二：obj 是指它(v-color="color" )后面设置的表达式
130     //{expression:"color",value:"red",}
131     Vue.directive("focus", {
132         inserted: function (el, obj) {
133             // console.log(el);
134             el.focus();
135         }
136     });
137     Vue.directive("color", {
138         inserted: function (el, obj) {
139             // obj.style.color = "red";
140             obj.style.color = obj.value;//????????????
141             console.log(obj.value);
142         }
143     });
144 
145     var vm = new Vue({
146         el: "#app",
147         data: {
148             delId:"",// 用来将要删除数据的id进行保存
149             isshow:false,
150             color: "green",
151             id: 0,
152             name: '',
153             list: [
154                 { "id": 1, "name": "itcast", "ctime": Date() },
155                 { "id": 2, "name": "黑马", "ctime": Date() }
156             ]
157         },
158         // mounted(){
159         //     this.getFocus()
160         // },
161         methods: {
162             add: function () {
163                 //将id和namepush到list数组中
164                 this.list.push({ "id": this.id, "name": this.name, "ctime": Date() });
165             },
166             del: function (id) {
167                 this.isshow = true;
168                 // 将得到的id保存到delId里面
169                 this.delId = id;    
170             },
171             beforeEnter:function(el) {
172                 el.style.left = "100%";
173             },
174             enter:function(el,done) {
175                 el.offsetHeight;
176                 el.style.left = "35%";                
177             },
178             afterEnter:function(el){
179                 
180             },
181             beforeLeave:function(el){
182                 el.style.left = "35%";
183             },
184             leave:function(el,done){
185                 el.offsetHeight;
186                 el.style.left = "100%";
187                 setTimeout(function(){
188                     done();
189                 },500);
190             },
191             afterLeave:function(el){
192                 
193             },
194             showClose:function(el){
195                 this.isshow = false;
196             },
197             delById:function() {
198                 _this = this;
199                 // 根据DelId删除对应的数据 
200                  var index = this.list.findIndex(function(item){
201                     return item.id ==_this.delId;
202                 });
203                 this.list.splice(index,1);
204                 // 关闭删除框
205                 this.isshow = false;
206             }
207         }
208     });
209 
210 </script>
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 