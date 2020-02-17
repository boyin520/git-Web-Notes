# CSS 样式计算临近颜色值？

原创[isea533](https://me.csdn.net/isea533) 发布于2019-01-05 17:55:44 阅读数 1080 [ 收藏]()

[展开]()

在有些场景下，有可能需要阶梯状的颜色展示，通过背景色对数据分组。或者就是自动生成某个颜色的临近值。如果颜色是固定的，直接计算好固定就行，如果是动态或者用户设置的颜色值，就需要一定的算法来计算临近的颜色。

我在看 MDN 时，发现了 RGB 和 HSL，发现 HSL 的值通过修改色调（hue）、饱和度（saturation）和明度（lightness）可以让颜色更有规律的进行渐变。

> 参考地址：
> <https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Introduction_to_CSS/Values_and_units#RGB>

**HSL** 简单介绍：

1. 色调：颜色的底色调。这个值在0到360之间，表示色轮的角度。
2. 饱和度：饱和度是多少？这需要一个从0-100%的值，其中0是没有颜色（它将显示为灰色），100%是全彩色饱和度。
3. 明度：颜色有多亮或明亮？这需要一个从0-100%的值，其中0是无光（它会出现全黑的），100%是充满光的（它会出现全白）。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190105173548852.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2lzZWE1MzM=,size_16,color_FFFFFF,t_70)

> 图片出处：<https://en.wikipedia.org/wiki/File:HSL_color_solid_cylinder_saturation_gray.png>

通过 HSL 的值的逐渐变化，可以得到非常相近的颜色。

既然通过 HSL 很容易计算，就搜搜如何在 RGB 和 HSL 之间进行转换。很容易就搜到了下面的网站：

> <http://www.easyrgb.com/en/math.php>

这个网站提供了多种颜色类型间的转换算法，这个算法基本上可以直接拿来当 JS 代码用，内部有些细节需要特殊处理。

针对颜色计算做了一个简单的封装，并且写了一些简单的方法来进行测试，下面是完整的代码，保存到 html 文件直接在浏览器打开即可。

> 直接访问：<https://abel533.github.io/color_rgb_hsl/>

[完整代码](https://github.com/abel533/color_rgb_hsl/blob/master/index.html)：

```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        var ColorUtil = (function () {
            function rgb2hsl({ R, G, B }) {
                var H, S, L;
                var var_R = (R / 255);
                var var_G = (G / 255);
                var var_B = (B / 255);

                var var_Min = Math.min(var_R, var_G, var_B)    //Min. value of RGB
                var var_Max = Math.max(var_R, var_G, var_B)    //Max. value of RGB
                var del_Max = var_Max - var_Min             //Delta RGB value

                var L = (var_Max + var_Min) / 2

                if (del_Max == 0)                     //This is a gray, no chroma...
                {
                    H = 0
                    S = 0
                }
                else                                    //Chromatic data...
                {
                    if (L < 0.5) S = del_Max / (var_Max + var_Min)
                    else S = del_Max / (2 - var_Max - var_Min)

                    var del_R = (((var_Max - var_R) / 6) + (del_Max / 2)) / del_Max
                    var del_G = (((var_Max - var_G) / 6) + (del_Max / 2)) / del_Max
                    var del_B = (((var_Max - var_B) / 6) + (del_Max / 2)) / del_Max

                    if (var_R == var_Max) H = del_B - del_G
                    else if (var_G == var_Max) H = (1 / 3) + del_R - del_B
                    else if (var_B == var_Max) H = (2 / 3) + del_G - del_R

                    if (H < 0) H += 1
                    if (H > 1) H -= 1
                }
                return { H, S, L };
            }

            function Hue_2_RGB(v1, v2, vH) {
                if (vH < 0) vH += 1;
                if (vH > 1) vH -= 1;
                if ((6 * vH) < 1) return (v1 + (v2 - v1) * 6 * vH);
                if ((2 * vH) < 1) return (v2);
                if ((3 * vH) < 2) return (v1 + (v2 - v1) * ((2 / 3) - vH) * 6);
                return v1;
            }

            function hsl2rgb({ H, S, L }) {
                var R, G, B;
                if (S == 0) {
                    R = L * 255;
                    G = L * 255;
                    B = L * 255;
                }
                else {
                    var var_2, var_1;
                    if (L < 0.5) var_2 = L * (1 + S);
                    else var_2 = (L + S) - (S * L);

                    var_1 = 2 * L - var_2;

                    R = Math.round(255 * Hue_2_RGB(var_1, var_2, H + (1 / 3)));
                    G = Math.round(255 * Hue_2_RGB(var_1, var_2, H));
                    B = Math.round(255 * Hue_2_RGB(var_1, var_2, H - (1 / 3)));
                }
                return { R, G, B };
            }

            function hex2rgb(hex) {
                if (hex.indexOf('#') != -1) {
                    hex = hex.substr(1, 6);
                }
                var R = parseInt(hex.substr(0, 2), 16);
                var G = parseInt(hex.substr(2, 2), 16);
                var B = parseInt(hex.substr(4, 2), 16);
                return { R, G, B };
            }

            function rgb2hex({ R, G, B }) {
                return '#' + dec2hex(R) + dec2hex(G) + dec2hex(B);
            }

            function dec2hex(dec) {
                return (dec + 0x100).toString(16).substr(1, 2).toUpperCase();
            }

            return {
                rgb2hsl,
                hsl2rgb,
                hex2rgb,
                rgb2hex
            }
        })();
    </script>
</head>

<body>
    <div class="main">
        <div class="form">
            <label for="hex">请输入 Hex 值: </label>
            <input type="text" id="hex">
            <button onclick="genColor()">生成</button>
        </div>
        <div class="title">改变色调H</div>
        <div class="container H">

        </div>
        <div class="title">改变饱和度S</div>
        <div class="container S">

        </div>
        <div class="title">改变明度L</div>
        <div class="container L">

        </div>
    </div>
    <script>

        function genColor(){
            document.querySelector('.H').innerHTML = genColorHtml('H', 0, 360);
            document.querySelector('.S').innerHTML = genColorHtml('S', 0, 100);
            document.querySelector('.L').innerHTML= genColorHtml('L', 0, 100);
        }

        function genColorHtml(type, min, max){
            var hex = document.getElementById('hex').value;
            var hsl = ColorUtil.rgb2hsl(ColorUtil.hex2rgb(hex));
            var hexArray = [];
            for (let i = min; i < max; i++) {
                hsl[type] = i/max;
                hexArray.push(ColorUtil.rgb2hex(ColorUtil.hsl2rgb(hsl)));
            }
            if (hex[0] != '#') {
                hex = '#' + hex;
            }
            hex = hex.toUpperCase();
            var html = '';
            for (let i = 0; i < hexArray.length; i++) {
                html += '<div class="item ' + (hexArray[i] == hex ? 'current' : '') + '" style="background-color:' + hexArray[i] + '"></div>';
            }
            return html;
        }
    </script>
    <style>
        .main {
            display: flex;
            flex-flow: column wrap;
            justify-content: space-between;
        }

        .container {
            display: flex;
        }

        .item {
            height: 50px;
            width: 50px;
        }

        .current {
            margin: 0 1px;
        }
    </style>
</body>

</html>
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174
```

在该页面可以输入不同的颜色预览效果，并且展示了分别修改 H,S,L 三种值的不同效果，例如输入 `ff00ff`，结果如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190105174448774.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2lzZWE1MzM=,size_16,color_FFFFFF,t_70)
上面的所有变化值都是1，所以颜色特别的接近，在实际使用中，为了更好的区分，可以增加变化值，例如 5 时的效果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190105174653208.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2lzZWE1MzM=,size_16,color_FFFFFF,t_70)

> 上述图片中，有白色边框的颜色是输入的 hex 值对应的颜色。

通过上面的算法根据自己的实际情况进行修改，就可以得到符合要求的临近色了。