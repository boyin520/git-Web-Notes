[Chrome 控制台console的用法](https://www.cnblogs.com/Leo_wl/p/4117446.html)

**阅读目录**<https://www.jb51.net/article/160096.htm>

- [Chrome 控制台console的用法](https://www.cnblogs.com/Leo_wl/p/4117446.html#_label0)

[回到目录](https://www.cnblogs.com/Leo_wl/p/4117446.html#_labelTop)

# [Chrome 控制台console的用法](http://www.cnblogs.com/ctriphire/p/4116207.html)

```
目前控制台方法和属性有：
["$$", "$x", "dir", "dirxml", "keys", "values", "profile", "profileEnd", "monitorEvents", "unmonitorEvents", "inspect", "copy", "clear", "getEventListeners", "undebug", "monitor", "unmonitor", "table", "$0", "$1", "$2", "$3", "$4", "$_"]

一般情况下我们用来输入信息的方法主要是用到如下四个
1、console.log 用于输出普通信息
2、console.info 用于输出提示性信息
3、console.error用于输出错误信息
4、console.warn用于输出警示信息
```

大家都有用过各种类型的浏览器，每种浏览器都有自己的特色，本人拙见，在我用过的浏览器当中，我是最喜欢Chrome的，因为它对于调试脚本及前端设计调试都有它比其它浏览器有过之而无不及的地方。可能大家对console.log会有一定的了解，心里难免会想调试的时候用alert不就行了，干嘛还要用console.log这么一长串的字符串来替代alert输出信息呢，下面我就介绍一些调试的入门技巧，让你爱上console.log

先的简单介绍一下chrome的控制台，打开chrome浏览器，按f12就可以轻松的打开控制台

![img](https://images0.cnblogs.com/blog/457824/201411/230844086569404.png)

大家可以看到控制台里面有一首诗还有其它信息，如果想清空控制台，可以点击左上角那个![img](https://images0.cnblogs.com/blog/457824/201411/230851146562060.png)来清空，当然也可以通过在控制台输入console.clear()来实现清空控制台信息。如下图所示

![img](https://images0.cnblogs.com/blog/457824/201411/230852535785272.png)

现在假设一个场景，如果一个数组里面有成百上千的元素，但是你想知道每个元素具体的值，这时候想想如果你用alert那将是多惨的一件事情，因为alert阻断线程运行，你不点击alert框的确定按钮下一个alert就不会出现。

下面我们用console.log来替换，感受一下它的魅力。

![img](https://images0.cnblogs.com/blog/457824/201411/230901063434461.png)

看了上面这张图，是不是认识到log的强大之处了，下面我们来看看console里面具体提供了哪些方法可以供我们平时调试时使用。

![img](https://images0.cnblogs.com/blog/457824/201411/230907017034998.jpg)

目前控制台方法和属性有：

```
["$$", "$x", "dir", "dirxml", "keys", "values", "profile", "profileEnd", "monitorEvents", "unmonitorEvents", "inspect", "copy", "clear", "getEventListeners", "undebug", "monitor", "unmonitor", "table", "$0", "$1", "$2", "$3", "$4", "$_"]
```

下面我们来一一介绍一下各个方法主要的用途。

一般情况下我们用来输入信息的方法主要是用到如下四个

**1、console.log** 用于输出普通信息

**2、console.info** 用于输出提示性信息

**3、console.error**用于输出错误信息

**4、console.warn**用于输出警示信息

用图来说话

![img](https://images0.cnblogs.com/blog/457824/201411/230913067037155.jpg)

**5、console.group**输出一组信息的开头

**6、console.groupEnd**结束一组输出信息

看你需求选择不同的输出方法来使用，如果上述四个方法再配合group和groupEnd方法来一起使用就可以输入各种各样的不同形式的输出信息。

![img](https://images0.cnblogs.com/blog/457824/201411/230916203758554.jpg)

哈哈，是不是觉得很神奇呀！

**7、console.assert**对输入的表达式进行断言，只有表达式为false时，才输出相应的信息到控制台

![img](https://images0.cnblogs.com/blog/457824/201411/230920471094898.jpg)

**8、console.count**（这个方法非常实用哦）当你想统计代码被执行的次数

![img](https://images0.cnblogs.com/blog/457824/201411/230922575009930.jpg)

**9、console.dir**(这个方法是我经常使用的 可不知道比for in方便了多少) 直接将该DOM结点以DOM树的结构进行输出，可以详细查对象的方法发展等等

![img](https://images0.cnblogs.com/blog/457824/201411/230936046711444.jpg)

**10、console.time** 计时开始

**11、console.timeEnd**  计时结束（看了下面的图你瞬间就感受到它的厉害了）

![img](https://images0.cnblogs.com/blog/457824/201411/230941441714991.jpg)

**12、console.profile**和**console.profileEnd**配合一起使用来查看CPU使用相关信息

![img](https://images0.cnblogs.com/blog/457824/201411/230947391254586.jpg)

在Profiles面板里面查看就可以看到cpu相关使用信息

![img](https://images0.cnblogs.com/blog/457824/201411/230948160786229.jpg)

**13、console.timeLine**和**console.timeLineEnd**配合一起记录一段时间轴

**14、console.trace**  堆栈跟踪相关的调试

上述方法只是我个人理解罢了。如果想查看具体API，可以上官方看看，具体地址为：https://developer.chrome.com/devtools/docs/console-api

 

下面介绍一下控制台的一些快捷键

**1、方向键盘的上下键**，大家一用就知晓。比如用上键就相当于使用上次在控制台的输入符号

**2、$_**命令返回最近一次表达式执行的结果，功能跟按向上的方向键再回车是一样的

![img](https://images0.cnblogs.com/blog/457824/201411/230958084845245.jpg)

上面的`$_`需要领悟其奥义才能使用得当，而0 4则代表了最近5个你选择过的DOM节点。

什么意思？在页面右击选择`审查元素`，然后在弹出来的DOM结点树上面随便点选，这些被点过的节点会被记录下来，而`$0`会返回最近一次点选的DOM结点，以此类推，$1返回的是上上次点选的DOM节点，最多保存了5个，如果不够5个，则返回`undefined`。

![img](https://images0.cnblogs.com/blog/431064/201409/132239377465002.gif)

**3、Chrome 控制台中原生支持类jQuery的选择器**，也就是说你可以用`$`加上熟悉的css选择器来选择DOM节点

![img](https://images0.cnblogs.com/blog/457824/201411/231001310469893.jpg)

**4、copy**通过此命令可以将在控制台获取到的内容复制到剪贴板

![img](https://images0.cnblogs.com/blog/457824/201411/231004464535964.jpg)

（哈哈 刚刚从控制台复制的body里面的html可以任意粘贴到哪 比如记事本  是不是觉得功能很强大）

**5、keys和values** 前者返回传入对象所有属性名组成的数据，后者返回所有属性值组成的数组

![img](https://images0.cnblogs.com/blog/457824/201411/231008342653765.jpg)

说到这，不免想起**console.table**方法了

![img](https://images0.cnblogs.com/blog/457824/201411/231012102186590.jpg)

 

 

## **6、monitor & unmonitor**

monitor(function)，它接收一个函数名作为参数，比如`function a`,每次`a`被执行了，都会在控制台输出一条信息，里面包含了函数的名称`a`及执行时所传入的参数。

而unmonitor(function)便是用来停止这一监听。

![img](https://images0.cnblogs.com/blog/457824/201411/231015303594113.jpg)

看了这张图，应该明白了，也就是说在monitor和unmonitor中间的代码，执行的时候会在控制台输出一条信息，里面包含了函数的名称`a`及执行时所传入的参数。当解除监视（也就是执行unmonitor时）就不再在控制台输出信息了。

```
$ // 简单理解就是 document.querySelector 而已。
$$ // 简单理解就是 document.querySelectorAll 而已。
$_ // 是上一个表达式的值
$0-$4 // 是最近5个Elements面板选中的DOM元素，待会会讲。
dir // 其实就是 console.dir
keys // 取对象的键名, 返回键名组成的数组
values // 去对象的值, 返回值组成的数组
```

 

下面看一下console.log的一些技巧

**1、重写console.log 改变输出文字的样式**

![img](https://images0.cnblogs.com/blog/457824/201411/231020045932166.png)

**2、利用控制台输出图片**

![img](https://images0.cnblogs.com/blog/431064/201409/132234240277278.gif)

**3、指定输出文字的样式**

![img](https://images0.cnblogs.com/blog/457824/201411/231028494216061.png)

 最后说一下chrome控制台一个简单的操作，如何查看页面元素，看下图就知道了

![img](https://images0.cnblogs.com/i/477954/201406/161255380974581.jpg)

你在控制台简单操作一遍就知道了，是不是觉得很简单！