### ||ajax  |  axios  |  promise  | fetch  |  jQuery ajax||

#### ajax 



------

#### axios

- 概念:    axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。
- 特点：
- 从浏览器中创建 XMLHttpRequests
- 从 node.js 创建 http 请求
- 支持 Promise API
- 拦截请求和响应
- 转换请求数据和响应数据
- 取消请求
- 自动转换 JSON 数据
- 客户端支持防御 XSRF  =>请求带一个cookie中key, 后台根据同源策略辨别假冒网站。


- 提供了一些并发请求的接口

*安装*：`npm i axios  -D`

```
axios({
    method: 'post',
    url: '/user/12345',
    data: {
        firstName: 'Fred',
        lastName: 'Flintstone'
    }
})
.then(function (response) {
    console.log(response);
})
.catch(function (error) {
    console.log(error);
});
```

vue现今写法：

```javascript
const vm = new Vue({
	el:'#app',
	data:{
	  categories:[ ]
	},
    created:function(){
        const that=this
        fetch('http://127.0.0.1:5500/api/index.json')
          .then(function(res){return res.json()})
          .then(function(data){that.categories = data})
    }
})
```



------

#### promise





-----

#### fetch

- 符合关注分离，没有将输入、输出和用事件来跟踪的状态混杂在一个对象里
- 更好更方便的写法，诸如：


- 更加底层，提供的API丰富（request, response）
- 脱离了XHR，是ES规范里新的实现方式

```
try {
  let response = await fetch(url);
  let data = response.json();
  console.log(data);
} catch(e) {
  console.log("Oops, error", e);
}
```



------

#### jQuery ajax

 ```	
$(function(){        
        var list = {}; //请求参数        
        $.ajax({           
            type : "POST",  //请求方式            
            contentType: "application/json;charset=UTF-8", //请求的媒体类型   
            url : "http://127.0.0.1/admin/list/", //请求地址           
            data : JSON.stringify(list),  //数据，json字符串            
            success : function(result) {  //请求成功
                console.log(result);
            },            
            error : function(e){  //请求失败，包含具体的错误信息
                console.log(e.status);
                console.log(e.responseText);
            }
        });
    });
 ```

————————————————

