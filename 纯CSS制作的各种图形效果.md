# 纯CSS制作的图形效果

# 斜切角

设计图含有斜切角的效果时，我们一般想到的方法是切出四个角为背景，然后用border连起来，这样就能显示出该效果了，那么直接使用css呢？下面就整理css做斜边的效果。

## 1、方案一：利用linear-gradient

```
.chamfer{
    background: #3b3; 
    background: linear-gradient(135deg, transparent 15px, #3b3 0) top left, 
			linear-gradient(-135deg, transparent 15px, #3b3 0) top right, 
			linear-gradient(-45deg, transparent 15px, #3b3 0) bottom right, 
			linear-gradient(45deg, transparent 15px, #3b3 0) bottom left; 	
    background-size: 50% 50%; 
    background-repeat: no-repeat;
}
</style>
<div class="chamfer" ></div>
```

效果如下：

## 2、方案二：利用clip-path

```
<style>
.base{
	width: 300px;height: 300px;
}	
.chamfer{
	background: #009EEB; 
       clip-path: polygon( 20px 0, calc(100% - 20px) 0, 100% 20px, 
			100% calc(100% - 20px), calc(100% - 20px) 100%, 
			20px 100%, 
			0 calc(100% - 20px), 
			0 20px
   		);
}
</style>
<div class="chamfer"></div>
```

效果如下：

## css曲线切口角的实现 

上面实现的2种切口是直线的，如何实现曲线切口角呢？下面就介绍利用radial-gradient实现曲线切口角：

```
<style>
.chamfer{
	background: #e72; 
        background: radial-gradient(circle at top left, transparent 15px, #e72 0) top left, 
			    radial-gradient(circle at top right, transparent 15px, #e72 0) top right, 
			    radial-gradient(circle at bottom right, transparent 15px,#e72 0) bottom right,
			    radial-gradient(circle at bottom left, transparent 15px, #e72 0) bottom left;
	background-size: 50% 50%; 
	background-repeat: no-repeat;
}
</style>
<div class="chamfer"></div>
```

效果如下：

## 使用Corner-shape

除了上面写的方法外，我们还可以使用插件Corner-shape来实现，Corner-shape这个插件很有意思，能够生成元素的角形状，比如圆角、反向圆角、矩形、直角的边角，使用SVG技术生成，使用上只需要设置预设的自定义属性，然后设置圆角边框的大小即可。 Corner-shape的使用链接：http://wenjiangs.com/wp-content/uploads/2017/06/corner-shape/

 例如：

```
corner-shape:bevel;
border-radius:10% / 30px;
width:400px;
height: 300px;
```



------







------

# [CSS画图形](https://www.w3cplus.com/blog/tags/112.html)

> **特别声明：**小站已开通年费VIP通道，年费价格为 **¥365.00元**。如果您喜欢小站的内容，可以点击[开通会员](https://www.zhi12.cn/paywall/membership/widget?uid=5491&cms=drupal&callback=https%3A//www.w3cplus.com&source=https%3A//www.w3cplus.com/css/masking-module-clipping.html&sign=847bb66c1015a6664e5da9fc098e4674)进行全站阅读。如果您对付费阅读有任何建议或想法，欢迎发送邮件至: **airenliao@gmail.com**!(^_^)

今天这个教程中主要和大家一起来探讨和学习：如何使用CSS来制作图形，比如说圆形，半圆形，三角形等。前面有几个教程中都有使用了纯CSS使用border来制作三角形，今天我特意在网上查阅了一下，介绍这样的教程还是蛮多的，因此我也决定整理一份相关教程出来与大家一起分享。最早发现使用border制作三角形是人物Eric Meyer的发现的，但我没有找到Eric Meyer介绍的教程，可相关教程就很多，我在此就不列出来了，我们开始今天的学习之旅吧。

#### 如何工作？

很少会有人意识到，当浏览器绘制的border，会有一个角度的问题。我们就是得用这样的一个技巧来制作三角的效果。我们只需要保证一边的边框是有色，其他边框色为透明色，这样我们就很容易制作出三角形，然后改变其大小来实现不同的效果。我们一起来看一段代码：

```
		.css-arrow-multicolor {
		  border-color: red green blue orange;
		  border-style:solid;
		  border-width:20px;
		  width:0;
		  height:0;
		}		
	
```

 

正如你看到的上面代码段是使用border制作的四个三角形，这些三角形都是直角三角形边界大小，如果你改变border-width的大小，你将得到的是另一个三角形

```
		.css-arrow-acute {
		  border-color: red green blue orange;
		  border-style:solid;
		  border-width:25px 10px 15px 30px;
		  width:0;
		  height:0;
		}
		
```

 

当你改变border-style时，你会发现一些很神的效果：

```
		border-style: dotted;
	
```

 

但这种创意在不同的浏览器下并是支持的。

下面我们一起来通过代码，看看不同类型的制作方法

#### 一、正方形（Square）

```
		#square { 
			width: 100px;
			height: 100px; 
			background: red; 
		}
	
```

正方形是最简单的了，只需要保证元素的宽度和高度相同，这样就ＯＫ了。当然我们还可以使用border直接绘制正方形，具体如何绘制大家可以动脑想想，我就不写了，不过使用border绘制正方形，里面不能填充内容的哟。

**效果：**

[![img](http://www.w3cplus.com/sites/default/files/shapes-1.png)](http://jsfiddle.net/w3cplus/aGX78/)

#### 二、长方形(Rectangle)

```
		#rectangle { 
			width: 200px; 
			height: 100px;
			background: red; 
		}
	
```

在正方形的基础上改变他们的大小，确保width和height值不相同就行了。

**效果：**

[![img](http://www.w3cplus.com/sites/default/files/shapes-2.png)](http://jsfiddle.net/w3cplus/aGX78/)

#### 三、圆形（Circle）

```
		#circle { 
			width: 100px; 
			height: 100px; 
			background: red; 
			-moz-border-radius: 50px; 
			-webkit-border-radius: 50px; 
			border-radius: 50px; 
		}
	
```

**效果：**

[![img](http://www.w3cplus.com/sites/default/files/shapes-3.png)](http://jsfiddle.net/w3cplus/aGX78/)

圆形的制作，我们采用的是[CSS3](http://www.w3cplus.com/css3)中的[border-radius](http://www.w3cplus.com/node/48)属性。在制作过程中，有几点需要注意，其一宽度和高度值相同，其二圆角值为宽度或高度值的一半。也有地方提使用设置圆角值为50%，但我在Webkit中有碰到过不支持百分数的情况。

#### 四、半圆形（Semicircle）

```
		#semicircle{ 
			width: 100px; 
			height: 50px; 
			background: red; 
			-moz-border-radius: 100px 100px 0 0; 
			-webkit-border-radius:  100px 100px 0 0; 
			border-radius:  100px 100px 0 0; 
		}
	
```

制作半圆和圆使用的方法是一样的，但需要配合元素的高度，宽度以及圆角的方位，制作出半圆形效果。

**效果：**

[![img](http://www.w3cplus.com/sites/default/files/shapes-4.png)](http://jsfiddle.net/w3cplus/aGX78/)

#### 五、扇形（Fan-Shaped）

```
		#fanShaped {
			background: none repeat scroll 0 0 red;
			-webkit-border-radius: 50px 0 0 0;
			-moz-border-radius: 50px 0 0 0;
			border-radius: 50px 0 0 0;
			height: 50px;
			width: 50px;
		}
	
```

扇形在这里也就是四分之一圆效果，在制作四分之一圆和制作半圆形一样的，我们需要配合的就是元素的三个属性值，具体大家可以参考上面的代码。

**效果：**

[![img](http://www.w3cplus.com/sites/default/files/shapes-5.png)](http://jsfiddle.net/w3cplus/aGX78/)

#### 六、椭圆形（Oval）

```
		#oval { 
			width: 200px; 
			height: 100px; 
			background: red; 
			-moz-border-radius: 100px / 50px; 
			-webkit-border-radius: 100px / 50px; 
			border-radius: 100px / 50px; 
		}
	
```

这里使用了border-radius的X/Y两轴取值，制作出一种变形的圆角，在配合宽度等值，就制作了类似椭圆形的一个效果。

**效果：**

[![img](http://www.w3cplus.com/sites/default/files/shapes-6.png)](http://jsfiddle.net/w3cplus/aGX78/)

#### 七、三角效果（Triangle）

教程起就是说的三角效果，这里不在说是如何实现的，我在这里列出几种常见的三角形代码，仅供大家参考

**1、三角朝上**

```
		#triangle-up { 
			width: 0; 
			height: 0; 
			border: 50px solid red;
			border-color: transparent transparent red;
	 }
	
```

border-bottom设置颜色

**2、三角朝下**

```
		#triangle-down { 
			width: 0; 
			height: 0; 
			border: 50px solid red;
			border-color: red transparent transparent;
	 }
	
```

border-top设置颜色

**3、三角向左**

```
			#triangle-left { 
				width: 0; 
				height: 0; 
				border: 50px solid red;
				border-color: transparent red transparent  transparent;
		 } 
		
```

border-right设置颜色

**4、三角向右**

```
			#triangle-right { 
				width: 0; 
				height: 0; 
				border: 50px solid red;
				border-color: transparent transparent transparent red;
		 }
	
```

border-left设置颜色

**5、左上三角形**

```
		#triangle-topleft { 
			width: 0; 
			height: 0; 
			border: 100px solid red; 
			border-color: red transparent transparent red; 
		}
	
```

设置顶部和左边的颜色值。

**6、右上三角**

```
		#triangle-topright { 
			width: 0; 
			height: 0; 
			border: 100px solid red; 
			border-color: red red transparent transparent; 
		}		
	
```

元素顶部和右边设置边框色

**7、左下三角**

```
		#triangle-bottomleft { 
			width: 0; 
			height: 0; 
			border: 100px solid red; 
			border-color: transparent transparent red red; 
		}		
	
```

元素底部和左边设置边框颜色

**8、右下三角**

```
		#triangle-bottomright { 
			width: 0; 
			height: 0; 
			border: 100px solid red; 
			border-color: transparent red  red transparent; 
		}
	
```

元素右边和底部设置边框颜色。

**效果：**

[![img](http://www.w3cplus.com/sites/default/files/shapes-7.png)](http://jsfiddle.net/w3cplus/aGX78/)

有关于三角形的制作，大家可以参考：《[Creating Triangles in CSS](http://jonrohan.me/guide/css/creating-triangles-in-css/)》、《[How to Create DIV Shapes Like Triangles and Circles](http://blog.mattforhire.ca/2011/08/10/how-to-create-div-shapes-like-triangles-and-circles/) 》、《[CSS三角形的方法](http://cssk8.com/html/css_Tutorial/201102/17-2585.html)》、《[Using borders to produce angled shapes](http://www.howtocreate.co.uk/tutorials/css/slopes)》等。

#### 八、平行四边形（Parallelogram）

```
		#parallelogram { 
			width: 150px;
			height: 100px; 
			-webkit-transform: skew(20deg); 
			-moz-transform: skew(20deg); 
			-o-transform: skew(20deg); 
			transform: skew(20deg);
			background: red; 
		}
	
```

平行四边形是在矩形的基础上运用了一个[CSS3](http://www.w3cplus.com/css3)的[transform](http://www.w3cplus.com/content/css3-transform)属性。使用了变形效果。

**效果：**

[![img](http://www.w3cplus.com/sites/default/files/shapes-8.png)](http://jsfiddle.net/w3cplus/aGX78/)

#### 九、六角星

```
		#star-six { 
			width: 0; 
			height: 0; 
			border-left: 50px solid transparent; 
			border-right: 50px solid transparent; 
			border-bottom: 100px solid red; 
			position: relative; 
		} 
		#star-six:after { 
			width: 0; 
			height: 0; 
			border-left: 50px solid transparent; 
			border-right: 50px solid transparent; 
			border-top: 100px solid red; 
			position: absolute; 
			content: ""; 
			top: 30px; 
			left: -50px; 
		}
	
```

这个六角星是使用了一个“:after”制作了另一个反方向的三角形，在定位层叠到一起，从而形成六角星，说白一点就是两个三角拼在一起变成了六角星。

**效果：**

[![img](http://www.w3cplus.com/sites/default/files/shapes-9.png)](http://jsfiddle.net/w3cplus/aGX78/)

#### 十、五角星

```
		#star-five { 
			margin: 50px 0; 
			position: relative;
			display: block; 
			color: red; 
			width: 0px; 
			height: 0px; 
			border-right: 100px solid transparent; 
			border-bottom: 70px solid red; 
			border-left: 100px solid transparent; 
			-moz-transform: rotate(35deg); 
			-webkit-transform: rotate(35deg); 
			-ms-transform: rotate(35deg); 
			-o-transform: rotate(35deg); 
			transform: rotate(35deg); 
		}
		 #star-five:before { 
			border-bottom: 80px solid red; 
			border-left: 30px solid transparent; 
			border-right: 30px solid transparent; 
			position: absolute; 
			height: 0; 
			width: 0; 
			top: -45px; 
			left: -65px; 
			display: block; 
			content: ''; 
			-webkit-transform: rotate(-35deg); 
			-moz-transform: rotate(-35deg); 
			-ms-transform: rotate(-35deg); 
			-o-transform: rotate(-35deg);
			transform: rotate(-35deg); 
			} 
			#star-five:after { 
				position: absolute; 
				display: block; 
				color: red; 
				top: 3px; 
				left: -105px; 
				width: 0px; 
				height: 0px; 
				border-right: 100px solid transparent; 
				border-bottom: 70px solid red; 
				border-left: 100px solid transparent; 
				-webkit-transform: rotate(-70deg); 
				-moz-transform: rotate(-70deg); 
				-ms-transform: rotate(-70deg); 
				-o-transform: rotate(-70deg); 
				transform: rotate(-70deg); 
				content: ''; 
				}
	
```

五角星制作，大家可以参考[Kit MacAllister](http://kitmacallister.com/author/kittrick/)写的《[CSS Only 5-Point Star](http://kitmacallister.com/2011/css-only-5-point-star/)》一文。

**效果：**

[![img](http://www.w3cplus.com/sites/default/files/shapes-10.png)](http://jsfiddle.net/w3cplus/aGX78/)

#### 十一、心形

```
		#heart { 
			position: relative; 
			width: 100px; 
			height: 90px; 
		} 
		#heart:before, 
		#heart:after { 
			position: absolute; 
			content: ""; 
			left: 50px; 
			top: 0; 
			width: 50px; 
			height: 80px; 
			background: red; 
			-moz-border-radius: 50px 50px 0 0;
			-webkit-border-radius: 50px 50px 0 0;
			 border-radius: 50px 50px 0 0; 
			-webkit-transform: rotate(-45deg); 
			-moz-transform: rotate(-45deg); 
			-ms-transform: rotate(-45deg); 
			-o-transform: rotate(-45deg); 
			transform: rotate(-45deg); -
			webkit-transform-origin: 0 100%; 
			-moz-transform-origin: 0 100%; 
			-ms-transform-origin: 0 100%; 
			-o-transform-origin: 0 100%; 
			transform-origin: 0 100%; 
			} 
			#heart:after { 
				left: 0; 
				-webkit-transform: rotate(45deg); 
				-moz-transform: rotate(45deg); 
				-ms-transform: rotate(45deg); 
				-o-transform: rotate(45deg); 
				transform: rotate(45deg); 
				-webkit-transform-origin: 100% 100%; 
				-moz-transform-origin: 100% 100%; 
				-ms-transform-origin: 100% 100%; 
				-o-transform-origin: 100% 100%; 
				transform-origin :100% 100%; 
			}
	
```

**效果：**

[![img](http://www.w3cplus.com/sites/default/files/shapes-11.png)](http://jsfiddle.net/w3cplus/aGX78/)

#### 十二、Pac-Man

```
			#pac-man { 
				width: 0px; 
				height: 0px;
				border: 60px solid red;
				border-color: red transparent red red ;
				-moz-border-radius: 60px;
				-webkit-border-radius: 60px;
				border-radius: 60px; 
			}
	
```

**效果：**

[![img](http://www.w3cplus.com/sites/default/files/shapes-12.png)](http://jsfiddle.net/w3cplus/aGX78/)

#### 十三、对话泡泡（Talk Bubble）

```
		#talkbubble { 
			width: 120px; 
			height: 80px; 
			background: red; 
			position: relative; 
			-moz-border-radius: 10px; 
			-webkit-border-radius: 10px; 
			border-radius: 10px; 
			} 
			#talkbubble:before { 
				content:"";
				position: absolute; 
				right: 100%; 
				top: 26px; 
				width: 0; 
				height: 0; 
				border-top: 13px solid transparent; 
				border-right: 26px solid red; 
				border-bottom: 13px solid transparent; 
			}
	
```

有关于更多的对话泡泡的制作，大家还可以参考[Nicolas](http://nicolasgallagher.com/)的《[Pure CSS speech bubbles](http://nicolasgallagher.com/pure-css-speech-bubbles/)》。

**效果：**

[![img](http://www.w3cplus.com/sites/default/files/shapes-13.png)](http://jsfiddle.net/w3cplus/aGX78/)

#### 十四、Point Burst

```
		#burst-12 { 
			background: red;
			width: 80px; 
			height: 80px; 
			position: relative; 
			text-align: center; 
		} 
		#burst-12:before, 
		#burst-12:after { 
			content: ""; 
			position: absolute; 
			top: 0; left: 0; 
			height: 80px; 
			width: 80px; 
			background: red; 
			} 
			#burst-12:before { 
				-webkit-transform: rotate(30deg); 
				-moz-transform: rotate(30deg); 
				-ms-transform: rotate(30deg); 
				-o-transform: rotate(30deg); 
				transform: rotate(30deg); 
				} 
				#burst-12:after { 
					-webkit-transform: rotate(60deg); 
					-moz-transform: rotate(60deg); 
					-ms-transform: rotate(60deg); 
					-o-transform: rotate(60deg); 
					transform: rotate(60deg); 
					}
	
```

**效果：**

[![img](http://www.w3cplus.com/sites/default/files/shapes-14.png)](http://jsfiddle.net/w3cplus/aGX78/)

#### 十五、阴阳图

```
		#yin-yang { 
			width: 96px;
			height: 48px; 
			background: #eee; 
			border-color: red; 
			border-style: solid; 
			border-width: 2px 2px 50px 2px; 
			border-radius: 100%; 
			position: relative; 
			} 
			#yin-yang:before { 
				content: ""; 
				position: 
				absolute; 
				top: 50%; 
				left: 0; 
				background: #eee; 
				border: 18px solid red; 
				border-radius: 100%; 
				width: 12px; 
				height: 12px; 
				} 
				#yin-yang:after { 
					content: ""; 
					position: absolute; 
					top: 50%; 
					left: 50%; 
					background: red; 
					border: 18px solid #eee; 
					border-radius:100%; 
					width: 12px; 
					height: 12px; 
				}
	
```

**效果：**

[![img](http://www.w3cplus.com/sites/default/files/shapes-15.png)](http://jsfiddle.net/w3cplus/aGX78/)

上面的图形都是彩用CSS或者部分采用了CSS3的属性制作出来的，是不是很有意思呀，如果你喜欢这样的教程，大家还可以点击[CSS3-Tricks](http://www.css-tricks.com/)提供的《[The Shapes of CSS](http://css-tricks.com/examples/ShapesOfCSS/)》里面展示了十多种图形的制作方法。由于部分图形效果使用了CSS3的部分属性，如果你还在使用IE的话，我建议你使用现代浏览器，比如：[Mozilla Firefox](http://www.mozilla-europe.org/en/firefox/)、[Google Chrome](http://www.google.com/chrome/)、[Safari](http://www.apple.com/safari/download/)、[Opera](http://www.opera.com/)。上面展示的效果可能部分实用性不大，但是使用css制作三角和圆有效果应用还是很多了，特别是用来制作tips效果。

如需转载烦请注明出处：**W3CPLUS**

著作权归作者所有。

商业转载请联系作者获得授权,非商业转载请注明出处。

原文: 

https://www.w3cplus.com/css/create-shapes-with-css

 © 

w3cplus.com

------



为了节省时间，下面图形都采用的一个标签，可以是块元素也可以是行内元素，不过行内元素需要加上“**display:block;**”，唯一不同的是，在此用了不同的类名来区别，相关类名我放在了标题的后面，以便大家对应查看。

#### 1、正方形（square）：

**CSS Code:**

```
		.square {
			width: 100px;
			height:100px;
			background: #E5C3B2;
		}
	
```

上面的方法是，设置宽度和高度一致就可以实现正方形的效果，下面展示一种boder制作正方形的效果：

```
		.square {
			width:0;
			height:0;
			border: 50px solid  #E5C3B2;/*边框大小等于正方形宽度（或高度）的一半*/
		}
	
```

**效果：**

![img](http://www.w3cplus.com/sites/default/files/201202/simple-shapes-1.png)

#### 2、平行四边形（parallelogram）

**CSS Code:**

```
		.parallelogram {
			width: 100px;
			height: 70px;
			-webkit-transform: skew(20deg);
			-moz-transform: skew(20deg);
			-o-transform: skew(20deg);
			-ms-transform: skew(20deg);
			transform: skew(20deg);
			background: #E5C3B2;
		}
	
```

**效果：**

![img](http://www.w3cplus.com/sites/default/files/201202/simple-shapes-2.png)

我们可以通过“skew”的值大小来控制角度，如果其值为负值，将会改变扭曲方向：

```
		.parallelogram2 {
			width: 100px;
			height: 70px;
			-webkit-transform: skew(-20deg);
			-moz-transform: skew(-20deg);
			-o-transform: skew(-20deg);
			-ms-transform: skew(-20deg);
			transform: skew(-20deg);
			background: #E5C3B2;
		}
	
```

**效果：**

![img](http://www.w3cplus.com/sites/default/files/201202/simple-shapes-3.png)

#### 3、菱形(diamond)

**CSS Code:**

```
		.diamond {
			width: 80px;
			height: 80px;
			margin: 40px 0 0 40px;
			-webkit-transform-origin: 0 100%;
			-moz-transform-origin: 0 100%;
			-o-transform-origin: 0 100%;
			-ms-transform-origin: 0 100%;
			transform-origin: 0 100%;
			-webkit-transform:rotate(-45deg);
			-moz-transform:rotate(-45deg);
			-o-transform:rotate(-45deg);
			-ms-transform:rotate(-45deg);
			transform:rotate(-45deg);
			background: #E5C3B2;
		}
	
```

**效果：**

![img](http://www.w3cplus.com/sites/default/files/201202/simple-shapes-4.png)

#### 4、长方形（）

**CSS Code:**

```
		.rectangle {
			width: 100px;
			height: 50px;
			background: #E5C3B2;
		}
	
```

**效果：**

![img](http://www.w3cplus.com/sites/default/files/201202/simple-shapes-5.png)

#### 5、梯形（trapezoid）

**梯形一**

**CSS Code:**

```
		.trapezoid-1 {
			height: 0;
			width: 100px;
			border-bottom: 100px solid #e5c3b2;
			border-left: 60px solid transparent;
			border-right: 60px solid transparent;
		}
	
```

**效果：**

![img](http://www.w3cplus.com/sites/default/files/201202/simple-shapes-6.png)

**梯形二**

**CSS Code:**

```
		.trapezoid-2 {
			height: 0;
			width: 100px;
			border-top: 100px solid #e5c3b2;
			border-left: 60px solid transparent;
			border-right: 60px solid transparent;
		}
	
```

**效果：**

![img](http://www.w3cplus.com/sites/default/files/201202/simple-shapes-7.png)

**梯形三**

**CSS Code:**

```
		.trapezoid-3 {
			height: 100px;
			width: 0;
			border-right: 100px solid #e5c3b2;
			border-top: 60px solid transparent;
			border-bottom: 60px solid transparent;
		}
	
```

**效果：**

![img](http://www.w3cplus.com/sites/default/files/201202/simple-shapes-8.png)

**梯形四**

**CSS Code:**

```
		.trapezoid-4 {
			height: 100px;
			width: 0;
			border-left: 100px solid #e5c3b2;
			border-top: 60px solid transparent;
			border-bottom: 60px solid transparent;
		}
	
```

**效果：**

![img](http://www.w3cplus.com/sites/default/files/201202/simple-shapes-9.png)

#### 6、三角形（triangle）

**三角形朝上**

**CSS Code:**

```
		.triangle-up {
			height: 0;
			width: 0;			
			border: 50px solid #e5c3b2;
			border-color: transparent transparent #e5c3b2 transparent;
		}		
	
```

**效果：**

![img](http://www.w3cplus.com/sites/default/files/201202/simple-shapes-10.png)

**三角朝右**

**CSS Code:**

```
		.triangle-rihgt {
			height: 0;
			width: 0;
			border: 50px solid #e5c3b2;
			border-color: transparent  transparent transparent #e5c3b2;
		}		
	
```

**效果：**

![img](http://www.w3cplus.com/sites/default/files/201202/simple-shapes-11.png)

**三角朝下**

**CSS Code:**

```
		.triangle-down {
			height: 0;
			width: 0;
			border: 50px solid #e5c3b2;
			border-color: #e5c3b2 transparent  transparent transparent;
		}		
	
```

**效果：**

![img](http://www.w3cplus.com/sites/default/files/201202/simple-shapes-12.png)

**三角朝左**

**CSS Code:**

```
		.triangle-left {
			height: 0;
			width: 0;
			border: 50px solid #e5c3b2;
			border-color:  transparent #e5c3b2 transparent transparent;
		}
	
```

**效果：**

![img](http://www.w3cplus.com/sites/default/files/201202/simple-shapes-13.png)

#### 7、半圆（semicircle）

**上半圆**

**CSS Code:**

```
		.semicircle-top {
			background: #e5c3b2;
			height: 25px;
			width: 50px;			
			-moz-border-radius: 50px 50px 0 0;
			-webkit-border-radius: 50px 50px 0 0;
			border-radius: 50px 50px 0 0;
		}		
	
```

**效果：**

![img](http://www.w3cplus.com/sites/default/files/201202/simple-shapes-14.png)

**右半圆**

**CSS Code:**

```
		.semicircle-right {
			background: #e5c3b2;
			height: 50px;
			width: 25px;			
			-moz-border-radius: 0 0px 50px 0;
			-webkit-border-radius:0 50px 50px 0;
			border-radius:0 50px 50px 0;
		}		
	
```

**效果：**

![img](http://www.w3cplus.com/sites/default/files/201202/simple-shapes-15.png)

**下半圆**

**CSS Code:**

```
		.semicircle-down {
			background: #e5c3b2;
			height: 25px;
			width: 50px;			
			-moz-border-radius:0 0 50px 50px;
			-webkit-border-radius:0 0 50px 50px;
			border-radius:0 0 50px 50px;
		}		
	
```

**效果：**

![img](http://www.w3cplus.com/sites/default/files/201202/simple-shapes-16.png)

**左半圆**

**CSS Code:**

```
		.semicircle-left {
			background: #e5c3b2;
			height: 50px;
			width: 25px;			
			-moz-border-radius:50px 0 0 50px;
			-webkit-border-radius:50px 0 0 50px;
			border-radius:50px 0 0 50px;
		}		
	
```

**效果：**

![img](http://www.w3cplus.com/sites/default/files/201202/simple-shapes-17.png)

#### 8、圆（circle）

**CSS Code:**

```
		.circle {
			background: #e5c3b2;
			height: 50px;
			width: 50px;			
			-moz-border-radius: 25px;
			-webkit-border-radius:25px;
			border-radius: 25px;
		}		
	
```

**效果：**

![img](http://www.w3cplus.com/sites/default/files/201202/simple-shapes-18.png)

#### 9、椭圆（oval）

**水平椭圆**

**CSS Code:**

```
		.ovalHor {
			background: #e5c3b2;
			height: 40px;
			width: 80px;			
			-moz-border-radius: 40px/20px;
			-webkit-border-radius:40px/20px;
			border-radius: 40px/20px;
		}		
	
```

**效果：**

![img](http://www.w3cplus.com/sites/default/files/201202/simple-shapes-19.png)

**垂直椭圆**

**CSS Code:**

```
		.ovalVert {
			background: #e5c3b2;
			height: 80px;
			width: 40px;			
			-moz-border-radius: 20px/40px;
			-webkit-border-radius:20px/40px;
			border-radius: 20px/40px;
		}		
	
```

**效果：**

![img](http://www.w3cplus.com/sites/default/files/201202/simple-shapes-20.png)

#### 10、表图（chartColorful）

**CSS Code:**

```
		.chartColorful {
			height: 0px;
			width: 0px;
			border: 50px solid red;
			border-color: purple red yellow orange;
			-moz-border-radius: 50px;
			-webkit-border-radius:50px;
			border-radius: 50px;
		}		
	
```

**效果：**

![img](http://www.w3cplus.com/sites/default/files/201202/simple-shapes-21.png)

#### 11、四分之一圆（quarterCircle）

**四分之一圆（上）**

**CSS Code:**

```
		.quarterCircleTop {
			background: #e5c3b2;
			height: 50px;
			width: 50px;			
			-moz-border-radius: 50px 0 0 0;
			-webkit-border-radius: 50px 0 0 0;
			border-radius: 50px 0 0 0;
		}		
	
```

**效果：**

![img](http://www.w3cplus.com/sites/default/files/201202/simple-shapes-22.png)

**四分之一圆（右）**

**CSS Code:**

```
		.quarterCircleRight {
			background: #e5c3b2;
			height: 50px;
			width: 50px;			
			-moz-border-radius: 0 50px 0 0;
			-webkit-border-radius: 0 50px 0 0;
			border-radius:0 50px 0 0;
		}			
	
```

**效果：**

![img](http://www.w3cplus.com/sites/default/files/201202/simple-shapes-23.png)

**四分之一圆（下）**

**CSS Code:**

```
		.quarterCircleBottom {
			background: #e5c3b2;
			height: 50px;
			width: 50px;			
			-moz-border-radius: 0  0 50px 0;
			-webkit-border-radius: 0  0 50px 0;
			border-radius:0  0 50px 0;
		}			
	
```

**效果：**

![img](http://www.w3cplus.com/sites/default/files/201202/simple-shapes-24.png)

**四分之一圆（左）**

**CSS Code:**

```
		.quarterCircleLeft {
			background: #e5c3b2;
			height: 50px;
			width: 50px;			
			-moz-border-radius: 0  0 0 50px;
			-webkit-border-radius: 0  0 0 50px;
			border-radius:0  0 0 50px;
		}			
	
```

**效果：**

![img](http://www.w3cplus.com/sites/default/files/201202/simple-shapes-25.png)

#### 12、Chart（quarterCircle）

**Chart（上）**

**CSS Code:**

```
		.chartTop {
			height: 0px;
			width: 0px;
			border:50px solid #e5c3b2;
			border-top-color: transparent;
			-moz-border-radius: 50px;
			-webkit-border-radius: 50px;
			border-radius: 50px;
		}		
	
```

**效果：**

![img](http://www.w3cplus.com/sites/default/files/201202/simple-shapes-26.png)

**Chart（右）**

**CSS Code:**

```
		.chartRight{
			height: 0px;
			width: 0px;
			border:50px solid #e5c3b2;
			border-right-color: transparent;			
			-moz-border-radius: 50px;
			-webkit-border-radius: 50px;
			border-radius: 50px;
		}			
	
```

**效果：**

![img](http://www.w3cplus.com/sites/default/files/201202/simple-shapes-27.png)

**Chart（下）**

**CSS Code:**

```
		.chartBottom {
			height: 0px;
			width: 0px;
			border:50px solid #e5c3b2;
			border-bottom-color: transparent;		
			-moz-border-radius: 50px;
			-webkit-border-radius: 50px;
			border-radius: 50px;
		}			
	
```

**效果：**

![img](http://www.w3cplus.com/sites/default/files/201202/simple-shapes-28.png)

**Chart（左）**

**CSS Code:**

```
		.chartLeft {
			height: 0px;
			width: 0px;
			border:50px solid #e5c3b2;
			border-left-color: transparent;			
			-moz-border-radius: 50px;
			-webkit-border-radius: 50px;
			border-radius: 50px;
		}			
	
```

**效果：**

![img](http://www.w3cplus.com/sites/default/files/201202/simple-shapes-29.png)

#### 13、心形（heart）

**左心形**

**CSS Code**

```
		.heartLeft{
			width: 0;
			height: 0;
			border-color: red;
			border-style: dotted;
			border-width: 0 40px 40px 0;
		}
	
```

**效果：**

![img](http://www.w3cplus.com/sites/default/files/201202/simple-shapes-30.png)

**右心形**

**CSS Code**

```
		.heartRight{
			width: 0;
			height: 0;
			border-color: red;
			border-style: dotted;
			border-width: 0  0 40px 40px;
		}
	
```

**效果：**

![img](http://www.w3cplus.com/sites/default/files/201202/simple-shapes-31.png)

#### 14、彩带（ribbon）

**CSS Code**

```
		.ribbon {
			width: 0;
			height: 100px;
			border-left: 50px solid red;
			border-right: 50px solid red;
			border-bottom: 35px solid transparent
		}
	
```

**效果：**

![img](http://www.w3cplus.com/sites/default/files/201202/simple-shapes-32.png)

上面就用CSS制作的32种图形效果，当然大家还可以发挥你的想像和创造，制作一些更精美的图形。

**扩展阅读：**

1. [The Shapes of CSS](http://css-tricks.com/examples/ShapesOfCSS/)
2. [Drawing with CSS3 Tips & little more](http://themegamag.com/coding/css-coding/drawing-with-css3-tips/)
3. [纯CSS制作的图形效果](http://www.w3cplus.com/css/create-shapes-with-css)

著作权归作者所有。

商业转载请联系作者获得授权,非商业转载请注明出处。

原文: 

https://www.w3cplus.com/css/css-simple-shapes-cheat-sheet

 © 

w3cplus.com