## What is jQuery?

jQuery is a fast, small, and feature-rich JavaScript library. It makes things like HTML document traversal and manipulation, event handling, animation, and Ajax much simpler with an easy-to-use API that works across a multitude of browsers. With a combination of versatility and extensibility, jQuery has changed the way that millions of people write JavaScript.

jQuery是一个快速，小型且功能丰富的JavaScript库。借助易于使用的API（可在多种浏览器中使用），使HTML文档的遍历和操作，事件处理，动画和Ajax等事情变得更加简单。兼具多功能性和可扩展性，jQuery改变了数百万人编写JavaScript的方式。

## A Brief Look简要介绍

### DOM Traversal and ManipulationDOM遍历和操纵

Get the `<button>` element with the class 'continue' and change its HTML to 'Next Step...'

获取`<button>`具有“ continue”类的元素，并将其HTML更改为“ Next Step ...”。

```
$( "button.continue" ).html( "Next Step..." )
```

### Event Handling事件处理

Show the `#banner-message` element that is hidden with `display:none` in its CSS when any button in `#button-container` is clicked.

单击任何按钮时`#banner-message`，显示隐藏`display:none`在CSS中的元素 `#button-container`。

```
var hiddenBox = $( "#banner-message" );
$( "#button-container button" ).on( "click", function( event ) {
  hiddenBox.show();
});
```



### Ajax

Call a local script on the server `/api/getWeather` with the query parameter `zipcode=97201` and replace the element `#weather-temp`'s html with the returned text.

`/api/getWeather` 使用查询参数在服务器上调用本地脚本，并将`zipcode=97201` 元素`#weather-temp`的html替换为返回的文本。

```
$.ajax({
  url: "/api/getWeather",
  data: {
    zipcode: 97201
  },
  success: function( result ) {
    $( "#weather-temp" ).html( "<strong>" + result + "</strong> degrees" );
  }
});
```

