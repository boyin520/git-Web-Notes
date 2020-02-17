# [session应用：验证用户是否已登录](https://www.cnblogs.com/luowenshuai/p/9347950.html)

node.js中使用session实现：验证用户是否已登录功能并维持用户的在线状态。

目的：用户必须登录才能进入商品页面和购物车页面，如果不经过登录就访问商品页面会拦截并自动跳转到登录页面。

用户登录后流程：![img](https://images2018.cnblogs.com/blog/1438144/201807/1438144-20180721202523058-374041567.png)                 

用户未登录流程：![img](https://images2018.cnblogs.com/blog/1438144/201807/1438144-20180721202646547-1712119142.png)

**为什么要用session：**Http协议是无状态的，也就导致服务器无法分辨是谁浏览了网页。为了维持用户在网站的状态，比如登陆、购物车等，出现了先后出现了四种技术，分别是隐藏表单域、URL重写、cookie、session。session和cookie都是用来解决Http协议不能维持状态的问题，但是session只存储在服务器端的，不会在网络中进行传输，所以较cookie来说，session相对安全一些。

**结构如图：**

![img](https://images2018.cnblogs.com/blog/1438144/201807/1438144-20180721205007451-261879932.png)

**一：模拟数据库user.json文件**：

```
{ "username" : "admin", "password" : "admin" }
```

**二：views目录存放两个ejs文件：**

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
//login.ejs文件:
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
</head>
<body>
    <form action="/loginDo" method="post">
        <ul>
            <li>用户名：<input type="text" name="username" /></li>
            <li>用户名：<input type="password" name="password" /></li>
            <li><input type="submit" value="登陆"/></li>
        </ul>
    </form>
</body>
</html>

//product.ejs文件:
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
</head>
<body>
    <p>欢迎您：<%=username%>浏览我的页面。<a href="/out">退出登录</a></p>
    <p>这是商品展示界面</p>
</body>
</html>
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

首先下载第三方模块

npm install express --save

npm install express-session --save

npm install body-parser --save

**三：核心app.js文件:**

（1）引入相关模块并设置中间件，设置模板引擎：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
var express = require("express");
var app = express();
var user = require("./user.json");//引入模拟数据库
//引入body-parser模块并设置中间件，以便用req.body来获取post的传值
var bodyParser = require("body-parser");
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({extended: true}));
//引入express-session模块并设置中间件
 var session = require("express-session");
app.use(session({
    secret : "nihao",  //加密session，随便写
    cookie : {maxAge : 60*1000*30}, //设置过期时间
    resave : true,  //强制保存session 默认为 true，建议设置成false
    saveUninitialized : false ////强制将未初始化的session存储 默认为true，建议设置成true
}));
app.set("view engine", "ejs");//设置模板引擎为ejs，默认从views目录下查找ejs文件
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

（2）设置用户登录页面路由

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
app.get("/login", function(req, res){
    res.render("login.ejs"); //渲染登录页面
});
app.post("/loginDo", function(req, res){
    var usernaem = req.body.username;
    var password = req.body.password;
    if( usernaem === user.username || password === user.password){
        req.session.userinfo = usernaem; //设置session，表示用户处在的登录状态
        //app.locals对象用于将数据传入所有的渲染ejs模板中，用<%=username%>接收
        req.app.locals["username"] = usernaem; 
        res.redirect("/product");
    }else{
        res.send("<script>alert('登陆失败！');location.href='/login';</script>");//用户名或密码不正确
    }
});
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

（3）使用session判断用户是否是登录状态，true则允许访问商品页面/false则跳转到登录页面

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
app.use( function(req,res,next){
    if( req.session.userinfo ){
        next();
    }else{
        res.send("<script>alert('您还没有登录，请先登录！');location.href='/login';</script>");
    }
} );
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

（4）设置商品页面路由，用户若想访问必需通过第三步的拦截

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
app.get("/product", function(req, res){
    res.render("product.ejs"); //渲染商品页面
});//退出登录
app.get( "/out", function( req, res ){
    req.session.destroy(); //销毁session，退出登录
    res.redirect("/login");
} );
//设置端口号
app.listen(8000, function(){
    console.log("success");
});
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

结束：

用户若没有登录直接访问“商品页面（/product）” ，就会进行拦截提示并跳转到“登录页面（/login）”，只有登录以后才能访问“商品页面”。

标签: [应用](https://www.cnblogs.com/luowenshuai/tag/%E5%BA%94%E7%94%A8/), [session](https://www.cnblogs.com/luowenshuai/tag/session/)