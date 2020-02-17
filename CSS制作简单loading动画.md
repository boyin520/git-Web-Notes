# [CSS制作简单loading动画](https://www.cnblogs.com/wz71014q/p/8862127.html)

　　曾经以为，loading的制作需要一些比较高深的web动画技术，后来发现大多数loading都可以用“障眼法”做出来。比如一个旋转的圆圈，并不都是将gif图放进去，有些就是画个静止图像，然后让它旋转就完了。gif图也可以，但是加载时间比较长。

　　CSS的animation可以做出大多数的loading，比如：

![img](https://images2018.cnblogs.com/blog/1337509/201804/1337509-20180417112710105-986696006.gif)![img](https://images2018.cnblogs.com/blog/1337509/201804/1337509-20180417112719323-1608890354.gif)![img](https://images2018.cnblogs.com/blog/1337509/201804/1337509-20180417112727247-828240559.gif)![img](https://images2018.cnblogs.com/blog/1337509/201804/1337509-20180417112735849-777338213.gif)

![img](https://images2018.cnblogs.com/blog/1337509/201805/1337509-20180522154629740-125601947.gif) 

# loading1：

1、HTML：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
1 <div id="ddr">
2     <div class="ddr ddr1"></div>
3     <div class="ddr ddr2"></div>
4     <div class="ddr ddr3"></div>
5     <div class="ddr ddr4"></div>
6     <div class="ddr ddr5"></div>
7 </div>
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

2、CSS：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 #ddr{
 2     margin: 0 auto;
 3     width: 70px;
 4     height: 120px;
 5 }
 6 .ddr{
 7     width: 10px;
 8     height: 120px;
 9     float: left;
10     margin: 2px;
11     background-color: #00ff00;
12     animation: loading 1s infinite ease-in-out;/*animation：动画名称 持续时间 动画速度曲线 延迟 执行多少次 是否正反方向轮流播放*/
13 }
14 .ddr2{
15     animation-delay: -0.9s;/*定义开始执行的地方，负号表示直接从第900ms开始执行*/
16 }
17 .ddr3{
18     animation-delay: -0.8s;
19 }
20 .ddr4{
21     animation-delay: -0.7s;
22 }
23 .ddr5{
24     animation-delay: -0.6s;
25 }
26 @keyframes loading {
27     0%,40%,100%{ /*定义每帧的动作*/
28         -webkit-transform: scaleY(0.5);
29     }
30     20%{
31         -webkit-transform: scaleY(1);
32     }
33 }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

# loading2：

1、HTML：

```
1 <div id="circle"></div>
```

2、CSS：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 #circle{
 2     margin: 20px auto;
 3     width: 100px;
 4     height: 100px;
 5     border: 5px white solid;
 6     border-left-color: #ff5500;
 7     border-right-color:#0c80fe;
 8     border-radius: 100%;
 9     animation: loading1 1s infinite linear;
10 }
11 @keyframes loading1{
12     from{transform: rotate(0deg)}to{transform: rotate(360deg)}
13 }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

# loading3：

1、HTML：

```
1 <div id="loader3">
2     <div id="loader3-inner"></div>
3 </div>
```

2、CSS：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 #loader3{
 2     box-sizing: border-box;
 3     position: relative;
 4     margin-left: 48%;
 5     transform: rotate(180deg);
 6     width: 50px;
 7     height: 50px;
 8     border: 10px groove rgb(170, 18, 201);
 9     border-radius: 50%;
10     animation: loader-3 1s ease-out alternate infinite;/* alternate表示则动画会在奇数次数（1、3、5 等等）正常播放，而在偶数次数（2、4、6 等等）反向播放 */
11 }
12 #loader3-inner{
13     box-sizing: border-box;
14     width: 100%;
15     height: 100%;
16     border: 0 inset rgb(170, 18, 201);
17     border-radius: 50%;
18     animation: border-zoom 1s ease-out alternate infinite;
19 }
20 @keyframes loader-3 {
21     0%{
22         transform: rotate(0deg);
23     }
24     100%{
25         transform: rotate(-360deg);
26     }
27 }
28 @keyframes border-zoom {
29     0%{
30         border-width: 0px;
31     }
32     100%{
33         border-width: 10px;
34     }
35 }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

# loading4：

1、HTML：

```
1 <div id="loading4">
2     <div id="loader4" class="heart"></div>
3 </div>
```

2、CSS：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 #loading4{
 2     width: 100%;
 3     height: 100px;
 4 }
 5 #loader4{
 6     position: relative;
 7     margin-left: calc(50% - 25px);
 8     width: 50px;
 9     height: 50px;
10     animation: loader-4 1s ease-in-out alternate infinite;
11 }
12 .heart:before{
13     position: absolute;
14     left: 11px;
15     content: "";
16     width: 50px;
17     height: 80px;
18     transform: rotate(45deg);
19     background-color: rgb(230, 6, 6);
20     border-radius: 50px 50px 0 0;
21 }
22 .heart:after{
23     position: absolute;
24     right: 11px;
25     content: "";
26     width: 50px;
27     height: 80px;
28     background-color: rgb(230, 6, 6);
29     transform: rotate(-45deg);
30     border-radius: 50px 50px 0 0;
31 }
32 @keyframes loader-4 {
33     0%{
34         transform: scale(0.2);
35         opacity: 0.5;
36     }
37     100%{
38         transform: scale(1);
39         opacity: 1;
40     }
41 }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

# 环形进度条：

0、原理：

 (1)、

![img](https://images2018.cnblogs.com/blog/1337509/201805/1337509-20180522164942709-242743634.png)

如图，先画一个正方形，这个正方形就是环形loading的轨道（现在还不是），再将一个圆放在上面，充当遮蔽物。

(2)、

![img](https://images2018.cnblogs.com/blog/1337509/201805/1337509-20180522165127149-447280671.png)

CSS3有个clip属性，可以裁剪图像，将上面的圆裁为一半，假如这个圆的右半部分一直看不见，旋转左边这个半圆，会出现怎样的效果呢？

(3)、

![img](https://images2018.cnblogs.com/blog/1337509/201805/1337509-20180522165401485-1831588845.png)![img](https://images2018.cnblogs.com/blog/1337509/201805/1337509-20180522165407243-227935240.png)

如图，就是这种效果，这时候再加一个遮罩，就形成了下面这种效果：

(4)、

![img](https://images2018.cnblogs.com/blog/1337509/201805/1337509-20180522165454540-1284002987.png)

再将最下面的正方形变成圆形（变成真正的轨道），旋转橙色部分的圆环，底部青色的就会露出来，就形成了进度条的效果

(5)、

![img](https://images2018.cnblogs.com/blog/1337509/201805/1337509-20180522165627504-54396499.png)

这是左边的一半，将右边的一半补齐：

(6)、

![img](https://images2018.cnblogs.com/blog/1337509/201805/1337509-20180522165844839-1018452531.png)

中间白色部分是mask，加上相应的文字，调整颜色就ok啦！

(7)、

![img](https://images2018.cnblogs.com/blog/1337509/201805/1337509-20180522170006181-1337263787.png)

1、HTML:

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 <!DOCTYPE html>
 2 <html lang="en">
 3 
 4 <head>
 5   <meta charset="UTF-8">
 6   <meta name="viewport" content="width=device-width, initial-scale=1.0">
 7   <meta http-equiv="X-UA-Compatible" content="ie=edge">
 8   <title>Document</title>
 9   <link rel="stylesheet" href="progress.css">
10 </head>
11 
12 <body>
13   <div class="circle">
14     <div class="preLeft">
15       <div class="left"></div>
16     </div>
17     <div class="preRight">
18       <div class="right"></div>
19     </div>
20   </div>
21   <div class="mask">
22     <span class="progress">0</span>%
23   </div>
24   <script src="progress.js"></script>
25 </body>
26 
27 </html>
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

2、CSS:

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 *{
 2   margin: 0;
 3   padding: 0;
 4 }
 5 .circle {
 6   width: 200px;
 7   height: 200px;
 8   border-radius: 50%;
 9   box-shadow: 0 0 7px 0px inset;
10   background:linear-gradient(#9ED110, #50B517, #179067, #476EAF, #9f49ac, #CC42A2, #FF3BA7, #FF5800, #FF8100, #FEAC00, #FFCC00, #EDE604);
11   filter: blur(8px);
12   transform: scale(1.1);
13   -webkit-mask: radial-gradient(black 30px, #0000 90px);
14 }
15 .preLeft{
16   position: absolute;
17   top: 0;
18   left: 0;
19   width: 200px;
20   height: 200px;
21   clip: rect(0, 100px, auto, 0);
22 }
23 .left{
24   position: absolute;
25   top: 0;
26   left: 0;
27   width: 200px;
28   height: 200px;
29   border-radius: 50%;
30   box-shadow: 0 0 3px 0px inset;
31   background: #fff;
32   transform: rotate(0deg);
33   clip: rect(0, 100px, auto, 0);
34 }
35 .preRight{
36   position: absolute;
37   top: 0;
38   left: 0;
39   width: 200px;
40   height: 200px;
41   clip: rect(0, auto, auto, 100px);
42 }
43 .right {
44   position: absolute;
45   top: 0;
46   left: 0;
47   width: 200px;
48   height: 200px;
49   border-radius: 50%;
50   box-shadow: 0 0 3px 0px inset;
51   background:#fff;
52   transform: rotate(0deg);
53   clip: rect(0, auto, auto, 100px);
54 }
55 .mask {
56   width: 150px;
57   height: 150px;
58   position: absolute;
59   left: 25px;
60   top: 25px;
61   border-radius: 50%;
62   /* box-shadow: 0 0 5px 0px; */
63   background: #FFF; 
64   text-align: center;
65   line-height: 150px;
66 }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

3、JS:

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 function init() {
 2   let left = document.getElementsByClassName('left')[0];
 3   let right = document.getElementsByClassName('right')[0];
 4   let progress = document.getElementsByClassName('progress')[0];
 5   let value = 0;
 6   let timer = setInterval(() => {
 7     if (progress.innerHTML < 100) {
 8       progress.innerHTML = value++;
 9       if (value <= 50) {
10         right.style.transform = 'rotate(' + (value * 3.6) + 'deg)';
11       } else if (value <= 100) {
12         left.style.transform = 'rotate(' + ((value - 50) * 3.6) + 'deg)';
13       }
14     } else {
15       clearInterval(timer);
16     }
17   }, 100);
18 }
19 window.onload = function () {
20   init();
21 };
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

\-

逢年过节，百度或者谷歌都会在首页播放一段动画，比如元宵节：

![img](https://images2018.cnblogs.com/blog/1337509/201804/1337509-20180417140603221-258437718.gif)

这个动画不仅仅是gif图，有时候是一张长长的包含所有帧的图片：

![img](https://images2018.cnblogs.com/blog/1337509/201804/1337509-20180417140801115-1154747364.png)

仿照animation一帧一桢的思路，可以将这张图片做成动画：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 #picHolder{
 2 /* 图片样式 */
 3     position: absolute;
 4     top: 17%;
 5     left: 50%;
 6     width: 270px;
 7     height: 129px;
 8     margin-left:-135px;
 9     background-image: url("../../../Library_image/tangyuan.png");
10     background-repeat: no-repeat;
11     background-position-x: 0;
12     cursor: pointer; 
13 }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 function animation () {
 2 /* 定时移动图片，使观众看到不同的帧 */
 3   var po = 0
 4   var i = 0
 5   var holder = document.getElementById('picHolder')
 6   setInterval(function () {
 7     po += -270
 8     i++
 9     holder.style.backgroundPositionX = po + 'px'
10     if (i >= 31) {
11       i = 0
12       po = 270
13     }
14   }, 100)
15 }
16 window.onload = function () {
17   animation()
18 }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

不过有时候他们又会放上一张gif图，不懂他们的套路

