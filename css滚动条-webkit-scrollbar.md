# css滚动条-webkit-scrollbar

2019-02-19 22:00:23

 

栈木地

 

阅读数 296

更多

分类专栏： [前端设计技巧](https://blog.csdn.net/amdd9582/article/category/8123848)

版权声明：本文为博主原创文章，遵循[ CC 4.0 BY-SA ](http://creativecommons.org/licenses/by-sa/4.0/)版权协议，转载请附上原文出处链接和本声明。本文链接：<https://blog.csdn.net/amdd9582/article/details/87734513>

介绍之前，先穿插一个小知识点，大神略过；

经常看见  在css属性前加上-webkit之类的代码，也就是兼容不同浏览器的代码；

-webkit 兼容chrome（谷歌）、safari（MacOS端浏览器）；

-moz 兼容firefox（火狐）；

-ms 兼容ie；

为什么讲这个，因为：修改滚动条样式只有谷歌支持；

所以：以下css属性直接根据需求带走；

```
::-webkit-scrollbar 滚动条整体，相当于包在最外面的父盒子



 



::-webkit-scrollbar-thumb  滚动条里面的小方块（可以刺溜刺溜移动的那个）



 



::-webkit-scrollbar-track  滚动条的轨道（你就理解成父盒子的背景）



 



::-webkit-scrollbar-button 滚动条的轨道的两端按钮



 



::-webkit-scrollbar-track-piece 内层轨道（就是track减去thumb剩余部分）



 



::-webkit-scrollbar-corner 边角，垂直和水平两个滚动条的交汇处



 



::-webkit-resizer 放在两个滚动条的交汇处，用于拖动调整元素大小的控件
```

**用法：**

我放一个滚动条demo，直接拉走就有效果（前提是兼容）；

```
<!DOCTYPE html>



<html lang="en">



<head>



    <meta charset="UTF-8" />



    <title>-webkit-scrollbar</title>



</head>



<style>



    /*先写好基础样式,可以忽略，为了更清楚显示效果*/



    .panel {



        width: 300px;



        height: 200px;



        margin: 0 auto;



        background: #242424;



        overflow-y: scroll;



    }



    .inner {



        height: 600px;



    }



    .inner li {



        height: 18%;



        margin: 2px 0;



        background: #009678;



    }



    /*从这里开始滚动条走起*/



    .panel::-webkit-scrollbar {



        width: 10px;



    }



    /*滑块*/



    .panel::-webkit-scrollbar-thumb {



        border-radius: 10px;



        background: #e0e0e0;



        -webkit-box-shadow: 0 0 5px #fff inset;



    }



    /*轨道*/



    .panel::-webkit-scrollbar-track {



        background: rgba(255, 255, 255, .3);



    }



    /*按钮*/



    .panel::-webkit-scrollbar-button {



        height: 10px;



        background: #81bd01;



    }



</style>



<body>



    <div class="panel">



        <div class="inner">



            <li>1</li>



            <li>2</li>



            <li>3</li>



            <li>4</li>



            <li>5</li>



        </div>



    </div>



</body>



</html>
```

说白了就是使用的伪元素实现的；