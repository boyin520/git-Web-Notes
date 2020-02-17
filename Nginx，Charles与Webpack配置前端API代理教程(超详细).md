# Nginx，Charles与Webpack配置前端API代理教程(超详细)

### 为什么前端需要配置API代理？

我们在开发一个项目的时候，如果服务采用的是分布式部署，也就是说按不同模块或功能部署于不同的服务器，如下图

![img](https://user-gold-cdn.xitu.io/2018/10/2/16632a7aa3bbb53b?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

![img](https://user-gold-cdn.xitu.io/2018/10/2/16632b5f9cd44e40?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

 

我们配置前端API的代理的目的其实就是为了解决跨域问题

### 配置前端API代理的三种方式

本文以配置Dva项目的代理为示例，因为Dva项目的脚手架自带Mock功能，省去了自己写接口的麻烦，同时公司内部项目也采用此技术，能让团队人员也做一参考。

示例地址：[github.com/ranshaw/fro…](https://github.com/ranshaw/frontEndProxy.git)

拉取代码，安装依赖之后，访问http://localhost:8000（默认是8000端口，如果被占用，会在其他端口），就能看到Welcome to dva的欢迎页面。项目中.roadhogrc.mock.js为Mock数据的配置文件，现为

![img](https://user-gold-cdn.xitu.io/2018/10/2/16632fda1c8e3366?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

 

http://localhost:8000/users/1

 

![img](https://user-gold-cdn.xitu.io/2018/10/2/16633a89d8b11a79?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

1. 使用www.frontproxy.com访问开发环境，即在浏览器输入www.frontproxy.com等同于输入localhost:8000。
2. 将users、todos、posts模块也就是请求路径中以/users/、/todos/、/posts/开始的请求接口，都代理到不同的服务器。
3. 最终实现请求如 [www.frontproxy.com/users/1](http://www.frontproxy.com/users/1) 返回的数据为请求[jsonplaceholder.typicode.com/users/1](http://jsonplaceholder.typicode.com/users/1) 返回的数据

注：本文中将三个模块的请求都代理到 [jsonplaceholder.typicode.com](http://jsonplaceholder.typicode.com/) 上；为了测试方便都采用Get请求，其他请求方式同Get方式；下文中讲述的配置方法以Mac为例，Windows上原理一致，具体方法需自行google。

### 配置Nginx和Hosts实现

##### 配置Hosts

在mac终端输入 `sudo vi /etc/hosts`，对Hosts文件进行编辑，添加如下配置 `127.0.0.1 www.frontendproxy.com`

![img](https://user-gold-cdn.xitu.io/2018/10/2/16633f58120b3add?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

Vim命令详解

现在我们访问www.frontproxy.com:8000和访问localhost:8000的效果是一样的，

![img](https://user-gold-cdn.xitu.io/2018/10/2/16633e168d0ca000?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

##### 配置Nginx

安装好Nginx之后，在Mac终端输入 `cd /usr/local/etc/nginx` 找到nginx.conf文件，可使用软件打开或者继续输入`vi nginx.conf` 添加如下配置

```
server {
        listen 80;
        server_name www.frontendproxy.com;
        index index.html;
        location / {
            proxy_pass http://127.0.0.1:8000/;
        }
    }
复制代码
```

在终端输入 `sudo nginx`，启动Nginx,此时在浏览器输入 [www.frontendproxy.com](http://www.frontendproxy.com/) 就可以正常显示了，但是当你修改页面的内容你会发现，[http://localhost:8000](http://localhost:8000/) 可以自动刷新，更新你刚才修改的内容，[www.frontendproxy.com](http://www.frontendproxy.com/) 页面并没有自动刷新，手动刷新后修改的内容才显示出来，并且页面中有一个报错

![img](https://user-gold-cdn.xitu.io/2018/10/2/16634048ab5c8b21?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

```
location /sockjs-node/ {
    proxy_pass http://127.0.0.1:8000/sockjs-node/;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";	
 }
复制代码
```

在终端输入`sudo nginx -s reload` 重启nginx,然后发现报错消失，修改页面内容后，可以自动刷新页面了。

到这一步，我们可以使用域名访问本地项目，进行愉快的开发了，但是请求users模块的数据仍然是Mock中的数据，

![img](https://user-gold-cdn.xitu.io/2018/10/2/166341285691ccd8?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

我们在Nginx中添加如下配置，对users模块的请求进行代理，

```
location /users/ {
    proxy_pass http://jsonplaceholder.typicode.com/users/;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
复制代码
```

在终端输入`sudo nginx -s reload`重启Nginx，再次请求 [www.frontendproxy.com/users/1](http://www.frontendproxy.com/users/1) 你会发现返回的数据已经不是Mock配置的数据了，而是 [jsonplaceholder.typicode.com/users/1](http://jsonplaceholder.typicode.com/users/1) 返回的数据，说明代理已经生效

![img](https://user-gold-cdn.xitu.io/2018/10/2/166341fc14249e36?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

同理对todos、posts模块进行配置，完整配置如下

```
server {
    listen 80;
    server_name www.frontendproxy.com;
    index index.html;
    location / {
        proxy_pass http://127.0.0.1:8000/;
    }
    location /sockjs-node/ {
        proxy_pass http://127.0.0.1:8000/sockjs-node/;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";	
     }
    location /users/ {
        proxy_pass http://jsonplaceholder.typicode.com/users/;
        proxy_set_header X-Real-IP $remote_addr;
	    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
    location /todos/ {
        proxy_pass http://jsonplaceholder.typicode.com/todos/;
        proxy_set_header X-Real-IP $remote_addr;
	    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
    location /posts/ {
        proxy_pass http://jsonplaceholder.typicode.com/posts/;
        proxy_set_header X-Real-IP $remote_addr;
	    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
复制代码
```

OK,我们现在已经完成了对users、todos、posts模块请求接口的代理，不同模块对应的服务器的地址，都可以自定义设置。

### Charles配置代理

Charles为Mac上一个超级好用的抓包软件，特别是对于移动端开发接口的调试，简直不要太爽，在此不做延伸，后续会专门写文章介绍,下载地址为： 链接: [pan.baidu.com/s/1UCJu9KaB…](https://pan.baidu.com/s/1UCJu9KaB3hgRXNG-UqYnDA) 提取码: shfp。

如果你按照上文教程配置了Hosts和Nginx，请把相关配置都清除掉，然后我们开始Charles的配置。

Charles并不能直接对Chrome进行抓包，需要对Chrome设置代理， 我这使用的是Chrome的一个插件SwitchyOmega，在SwitchOmega中配置代理，8888为Charles默认的端口，如果你更改过，请填写你更改后的端口

![img](https://user-gold-cdn.xitu.io/2018/10/2/166343b6f4e81978?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

![img](https://user-gold-cdn.xitu.io/2018/10/2/1663441390b8ed9f?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

![img](https://user-gold-cdn.xitu.io/2018/10/2/1663443854a09c82?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

![img](https://user-gold-cdn.xitu.io/2018/10/2/1663501dc84408e3?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

点击add按钮进行添加

![img](https://user-gold-cdn.xitu.io/2018/10/2/16635030ece24a28?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

 

www.frontendproxy.com

 

![img](https://user-gold-cdn.xitu.io/2018/10/2/166350a2f2cb2d4b?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

![img](https://user-gold-cdn.xitu.io/2018/10/2/166350eec785b407?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### Wepack + Host Switch Plus配置

现在前端开发环境中基本上都有用到webpack,其中webpack-dev-server这个包也是使用webpack搭建开发环境所必备的，我们常用的自动刷新和热更新功能就是由它提供的，今天所要讲的proxy功能也是如此。

Host Switch Plus，顾名思义就是对host的管理工具，它是一个Chrome的插件，需要你在Chrome应用商店下载。

首先我们先配置对域名的代理，支持单个添加和批量添加

![img](https://user-gold-cdn.xitu.io/2018/10/2/16635302eccab4c1?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

![img](https://user-gold-cdn.xitu.io/2018/10/2/16635311e9b53f65?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

 

www.frontendproxy.com已经代理到

 

http://localhost:8000

 

```
{
  "proxy": {
  "/sockjs-node/": {
    "target": "http://127.0.0.1:8000/",
    "changeOrigin": true
  },
  "/users/": {
    "target": "http://jsonplaceholder.typicode.com/",
    "changeOrigin": true
  },
  "/todos/": {
    "target": "http://jsonplaceholder.typicode.com/",
    "changeOrigin": true
  }
}
}
复制代码
```

好了，关于webpack配置代理的操作，到这里也完成了，关于[webpack-dev-server](https://webpack.docschina.org/configuration/dev-server/)中proxy配置的详细规则，请点击查看，我这里不再一一讲述。

**注意：** 如果你现在使用的是Nginx配置的代理，现在想转为webpack配置代理，假如要代理的模块为`/users/`,期望代理的接口地址为`http://localhost:8000/login`,Nginx和Webpack的配置分别如下，最终代理服务器发出的请求为`http://jsonplaceholder.typicode.com/login`,Webpack的配置项要多加一项配置`pathRewrite`，对代理服务器的路径进行重写，将请求中的`/users/`替换为`/`,有时候后端会需要请求头中的某些值，这时候就需要添加自定义请求头（版本webpack-dev-server@3.1.7），如下：

```
 Nginx配置：
 location /users/ {
        proxy_pass http://jsonplaceholder.typicode.com/;
        proxy_set_header X-Real-IP $remote_addr;
	    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
 Webpack配置：
 {
  "proxy": {
  "/users/": {
    "target": "http://jsonplaceholder.typicode.com/",
    "changeOrigin": true，
    "pathRewrite": { "^/users/": "/" }，
    "headers":{
       // 你需要定义的请求头
       Host: "localhost:8000"
    }
  },
}
}
复制代码
```

### 总结

下面是划重点时间，我们从适用范围、配置难易、团队协作成本三个方面对比一下这三种配置方式

![img](https://user-gold-cdn.xitu.io/2018/10/2/166355df15611831?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

