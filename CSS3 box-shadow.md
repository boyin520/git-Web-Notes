# CSS3 box-shadow

 属性用来描述一个元素的一个或多个阴影效果，该属性几乎可以让你完成你想要的任何阴影效果。然而 box-shadow 属性语法和取值非常灵活，对于新手有点不容易理解。今天总结一下语法和 box-shadow 属性各种阴影效果。

### 语法

CSS 代码:

```
/* offset-x | offset-y | color */box-shadow: 60px -16px teal; /* offset-x | offset-y | blur-radius | color */box-shadow: 10px 5px 5px black; /* offset-x | offset-y | blur-radius | spread-radius | color */box-shadow: 2px 2px 2px 1px rgba(0, 0, 0, 0.2); /* inset | offset-x | offset-y | color */box-shadow: inset 5em 1em gold; /* Any number of shadows, separated by commas */box-shadow: 3px 3px red, -1em 0 0.4em olive; /* Global keywords */box-shadow: inherit;box-shadow: initial;box-shadow: unset;
```

取值说明：

- `inset`: 默认阴影在边框外。使用 inset 后，阴影在边框内（即使是透明边框），背景之上内容之下。也有些人喜欢把这个值放在最后，浏览器也支持。
- `<offset-x> <offset-y>`: 这是头两个 `<length>`值，用来设置阴影偏移量。`<offset-x>` 设置水平偏移量，如果是负值则阴影位于元素左边。 `<offset-y>` 设置垂直偏移量，如果是负值则阴影位于元素上面。可用单位请查看 `<length>`。如果两者都是0，那么阴影位于元素后面。这时如果设置了 `<blur-radius>` 或 `<spread-radius>` 则有模糊效果。
- `<blur-radius>`: 这是第三个 `<length>` 值。值越大，模糊面积越大，阴影就越大越淡。 不能为负值。默认为0，此时阴影边缘锐利。
- `<spread-radius>` : 这是第四个 `<length>` 值。取正值时，阴影扩大；取负值时，阴影收缩。默认为0，此时阴影与元素同样大。
- `<color>` : 相关事项查看 `<color>` 。如果没有指定，则由浏览器决定——通常是color的值，不过目前Safari取透明。

网上找了几张图，大家可以对应的看一下，更加好理解。

![CSS3 box-shadow 属性](https://www.html.cn/newimg88/2018/07/syntax-1.png)
![CSS3 box-shadow 属性](https://www.html.cn/newimg88/2018/07/syntax-2.png)
![CSS3 box-shadow 属性](https://www.html.cn/newimg88/2018/07/syntax-3.png)

再说的具体一点：

CSS 代码:

```
div {    width: 150px;    height: 150px;    background-color: #fff;        box-shadow: 120px 80px 40px 20px #0ff;    /* 顺序为: offset-x, offset-y, blur-size, spread-size, color */    /* blur-size 和 spread-size 是可选的 (默认为 0) */}
```

来个图解：

![CSS3 box-shadow 属性图解](https://www.html.cn/newimg88/2018/07/box-shadow-diagram.png)

### 最简单的常规效果

下面是一些最简单的阴影效果，看代码也应该非常容易理解：

### 单边阴影效果

单边阴影效果可以做一些效果，比如特殊场景下描边，小阴影，再比如一些过渡色。

### 双边边阴影及多重阴影效果

### 其他一些有意思的阴影：

使用伪元素::before和::after，我们能创造出非常逼真的只有图片才能实现的阴影效果。让我来看一个例子：

再来一些效果：

再来看一些美轮美奂的效果：<https://codersblock.com/blog/creating-glow-effects-with-css/>

部分示例来自：<http://webexpedition18.com/articles/css-box-shadow-property/>

**相关推荐**

1.[Css不规则阴影怎么设置](https://www.html.cn/qa/css3/13128.html)

2.[前端开发技术文章](https://www.html.cn/web/)

3.[CSS3常见问题](https://www.html.cn/qa/css3/)