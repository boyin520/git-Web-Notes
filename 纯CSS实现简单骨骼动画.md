# 纯CSS实现简单骨骼动画

## 1 背景

某天设计师来找我说，“这个心愿牌傻傻地挂在那不好看，加个动效呗，就左右摆动一下就行，很简单的！”，我一想，行呀，提升用户视觉体验，开搞。

![心愿牌](https://user-gold-cdn.xitu.io/2019/12/7/16edf1ef1aac2248?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

十分钟后，🥶不对呀，这个左右摆动好假，不像现实中风吹的效果。

> 注：此处加快了动画的速度和摆动的幅度。

```
.animate-1 {
    animation: swing1 1s ease-in-out infinite;
    transform: rotate(-5deg);
    transform-origin: top center;
}
@keyframes swing1 {
    0% { transform: rotate(-5deg); }
    50% { transform: rotate(5deg);}
    100% { transform: rotate(-5deg);}
}
复制代码
```

![整体摆动](https://user-gold-cdn.xitu.io/2019/12/7/16edf33831e2fec8?imageslim)

冷静思考🤔，为啥这个摆动会没有灵魂。于是拿起工卡开始摆动，看看现实中的摆动效果是咋样的，最后豁然开朗：原来现实中的心愿牌（和工卡同理）在受力的时候，并不会整体摆动，而是会根据节点位置分成几部分有关联地摆动，这其实是个简单的骨骼动画！那到底怎么去实现呢？

## 2 骨骼动画

关于前端骨骼动画的实现可以参考[《骨骼动画原理与前端实现浅谈》](https://xieguanglei.github.io/blog/post/skeleton.html)，里面简单提及了css和canvas两种实现方式。这里将以这个心愿牌摆动动画为例，和大家一起研究如何使用css来实现。

### 2.1 分离元素

要让动画元素分开运动，首先需要拆分元素。拆分的依据是上面提到的节点，也就是骨骼动画中所谓的关节。例如这个心愿牌就有两个关节，分别在牌的上面和牌的下面，于是我们能拆分出3个动画元素：

![元素拆分](https://user-gold-cdn.xitu.io/2019/12/7/16edf46e72482372?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 2.2 拼接元素

```
<div>
    <!--元素1-->
    <div class="item-1"></div>
    <!--元素2-->
    <div class="item-2"></div>
    <!--元素3-->
    <div class="item-3"></div>
</div>
复制代码
```

> 这里看似简单，但是如果对骨骼动画不了解的话，会掉到坑里，上面就是错误的示范。为了加深大家的理解，特地挖了一个坑😆。

### 2.3 添加动效

在上面的基础上，我们就可以为每一部分添加不同幅度和方向的摆动动效啦。

```
<div class="animate-2">
    <!--元素1-->
    <div class="item-1"></div>
    <!--元素2-->
    <div class="item-2"></div>
    <!--元素3-->
    <div class="item-3"></div>
</div>
复制代码
```

```
.animate-2 .item-1 {
    /* 设置margin是为了定位，使其部分重叠在一起 */
    margin-bottom: -8px;
    margin-left: 18px;
    position: relative;
    z-index: 1;
    animation: swing2-1 1s ease-in-out infinite;
    transform: rotate(-3deg);
    transform-origin: top center;
}
.animate-2 .item-2 {
    animation: swing2-2 1s ease-in-out infinite;
    transform: rotate(5deg);
    transform-origin: top center;
}
.animate-2 .item-3 {
    margin-top: -5px;
    margin-left: 17.5px;
    position: relative;
    animation: swing2-3 1s ease-in-out infinite;
    transform: rotate(-5deg);
    transform-origin: top center;
}
@keyframes swing2-1 {
    0% { transform: rotate(-3deg); }
    50% { transform: rotate(3deg);}
    100% { transform: rotate(-3deg);}
}
@keyframes swing2-2 {
    0% { transform: rotate(5deg); }
    50% { transform: rotate(-5deg);}
    100% { transform: rotate(5deg);}
}
@keyframes swing2-3 {
    0% { transform: rotate(-5deg); }
    50% { transform: rotate(5deg);}
    100% { transform: rotate(-5deg);}
}
复制代码
```

大功告成😼？来看一下效果吧：

![分开摆动](https://user-gold-cdn.xitu.io/2019/12/7/16edf59a84e11caf?imageslim)

我的天😮，这是啥啊！！！看起来摆动确实要比整体摆动真实，不同元素有不同的摆动幅度和方向。但是错位了呀。

再继续冷静思考🤔，问题出在，骨骼动画的每一个子动画都是有关联的，而我们上面设计的每一个动画都是独立的。举个例子，顶部的红绳摆动时，会牵引住下面的牌子，导致下面牌子位置发生变化。下面的牌子在位置发生变化的同时，播放自己摆动的动画，这才是骨骼动画！

### 2.4 填坑 - 从js实现骨骼动画来理解其原理

这里又给大家推荐个学习资料：[《coding-math》系列 - 用数学知识和canvas创造好玩的动画](https://www.youtube.com/user/codingmath)，其中[这一集](https://www.youtube.com/watch?v=4oCo1j8xGew)讲解了骨骼动画的原理，对应的源码在[这里](https://github.com/bit101/CodingMath/tree/master/episode44)，因为在油管，为了避免某些同学没能科学上网看不到，所以以下面的跑步动作为例，讲解一下js实现过程:

1. 根据大腿的初始状态，当前旋转速度，计算大腿下一帧的位置；
2. 根据当前大腿的位置和小腿当前的速度，计算小腿下一帧的位置；
3. ...无限循环...

![coding-math](https://user-gold-cdn.xitu.io/2019/12/7/16edf668ed408375?imageslim)

从这里可以看出，小腿的位置是依赖大腿的位置的，这就不会出现我们上面的错位情况。所以说白了，骨骼动画的特性就是：

**关键元素带着子元素一起运动，子元素在此基础上自己运动。**

那么js实现中是通过先计算大腿位置，再由大腿位置计算小腿位置来实现联结的，而css该怎么实现呢？

### 2.5 纯css实现

回顾最关键的地方：**关键元素带着子元素一起运动，子元素在此基础上自己运动。**，要实现关键元素和子元素一起运动，在css里面，只要关键元素包裹子元素即可！，这就是css实现骨骼动画的基石。

```
<div class="animate-3">
    <!--运动模块1-->
    <div class="s-1">
        <div class="item-1"></div>
        <!--运动模块2-->
        <div class="s-2">
            <div class="item-2"></div>
            <!--运动模块3-->
            <div class="s-3">
                <div class="item-3"></div>
            </div>
        </div>
    </div>
</div>
复制代码
```

```
.animate-3 .s-1 {
    animation: swing3-1 1s ease-in-out infinite;
    transform: rotate(-3deg);
    transform-origin: top center;
}
.animate-3 .s-2 {
    animation: swing3-2 1s ease-in-out infinite;
    transform: rotate(-5deg);
    transform-origin: top center;
}
.animate-3 .s-3 {
    animation: swing3-3 1s ease-in-out infinite;
    transform: rotate(-5deg);
    transform-origin: top center;
}
@keyframes swing3-1 {
    0% { transform: rotate(-3deg); }
    50% { transform: rotate(3deg);}
    100% { transform: rotate(-3deg);}
}
@keyframes swing3-2 {
    0% { transform: rotate(5deg); }
    50% { transform: rotate(-5deg);}
    100% { transform: rotate(5deg);}
}
@keyframes swing3-3 {
    0% { transform: rotate(-5deg); }
    50% { transform: rotate(5deg);}
    100% { transform: rotate(-5deg);}
}
复制代码
```

![骨骼动画摆动](https://user-gold-cdn.xitu.io/2019/12/7/16edf74bd76e39aa?imageslim)

这次终于大功告成了👍。这里有三个元素，更多元素也是同理的，不断嵌套即可。

## 3. 最终动效演示

细心的同学会发现上面实现的骨骼动画看着也别扭，归根结底是各个元素摆动的方向和幅度没有调节好，这里附上调整完的效果，用心感受：

```
.animate-4 .s-1 {
    animation: swing4-1 5s ease-in-out infinite;
    transform: rotate(-2deg);
    transform-origin: top center;
}
.animate-4 .s-2 {
    animation: swing4-2 8s ease-in-out infinite;
    transform: rotate3d(0, 1, 0, 20deg);
    transform-origin: top center;
}
.animate-4 .s-3 {
    animation: swing4-3 8s ease-in-out infinite;
    transform: rotate(3deg);
    transform-origin: top center;
}
@keyframes swing4-1 {
    0% { transform: rotate(-2deg); }
    50% { transform: rotate(2deg);}
    100% { transform: rotate(-2deg);}
}
@keyframes swing4-2 {
    0% { transform: rotate3d(0, 1, 0, 20deg); }
    50% { transform: rotate3d(0, 1, 0, -20deg);}
    100% { transform: rotate3d(0, 1, 0, 20deg);}
}
@keyframes swing4-3 {
    0% { transform: rotate(3deg); }
    50% { transform: rotate(-3deg);}
    100% { transform: rotate(3deg);}
}
复制代码
```

![骨骼动画-调整后](https://user-gold-cdn.xitu.io/2019/12/7/16edf81ffa9f593a?imageslim)

## 4. End

纯CSS确实能实现骨骼动画，但仅限于简单的场景。在复杂场景中，例如前端游戏里面的骨骼动画，涉及到的节点比较多，用CSS虽然能实现，但效率不高，所以社区有很多从设计工具直接导出可用的骨骼动画信息，再用js来加载运行的方案，大家感兴趣可以Google一下。

本文主要通过简单的案例来加深大家对骨骼动画的原理性的认识，至于最后大家用CSS还是用JS来实现，就是“杀鸡要不要用牛刀”的问题了。

个人认为，只要屠龙刀在手，用不用已经不重要了。加油💪，希望大家能在各个方向找到自己的屠龙刀。