# doodoo.js快速入门教程

分类：快应用

[node](https://ask.dcloud.net.cn/topic/node) [nodejs](https://ask.dcloud.net.cn/topic/nodejs) [支付宝小程序](https://ask.dcloud.net.cn/topic/%E6%94%AF%E4%BB%98%E5%AE%9D%E5%B0%8F%E7%A8%8B%E5%BA%8F) [微信小程序](https://ask.dcloud.net.cn/topic/%E5%BE%AE%E4%BF%A1%E5%B0%8F%E7%A8%8B%E5%BA%8F) [小程序](https://ask.dcloud.net.cn/topic/%E5%B0%8F%E7%A8%8B%E5%BA%8F)

### 简介

 Doodoo.js -- 中文最佳实践Node.js快速开发框架。支持Koa.js, Express.js中间件，支持模块机制，插件机制，钩子机制，让开发 Node.js 项目更加简单、高效、灵活。

### 特性

支持koa全部中间件
支持使用 ES6+ 全部特性来开发项目
支持断点调试 ES6+ 项目
支持多种项目结构和多种项目环境
支持 Route, Controller 中使用Koa.js的所有API
支持多级 Controller
支持模块化开发
支持钩子机制
支持插件机制
支持错误处理
支持全局 doodoo 变量
支持 mysql, mongodb 数据库
支持前置，后置操作
支持 Restful 设计
支持启动自定义
支持环境加载配置
...

### 安装

环境要求：node >= 7.6.0

```
复制代码//npm  
npm install doodoo.js --save  
//yarn  
yarn add doodoo.js
```

### 使用 ES6/7 特性来开发项目

```
复制代码//base controller, app/demo/controller/base.js  
module.exports = class extends doodoo.Controller {  

    async _initialize() {  
        console.log('base _initialize');  
    }  

    async isLogin() {  
        console.log('base isLogin');  
    }  
}  

//index controller, app/demo/controller/index.js  
const base = require('./base');  
module.exports = class extends base {  

    async _initialize() {  
        await super._initialize();  
    }  

    async index() {  
        this.success("Hello Doodoo.js");  
    }  

    async index2() {  
        this.fail("Hello Doodoo.js");  
    }  
}
```

### 详细的日志

**服务 启动日志**

```
复制代码[doodoo] Version: 2.0.0  
[doodoo] Website: 127.0.0.1  
[doodoo] Nodejs Version: v8.12.0  
[doodoo] Nodejs Platform: darwin x64  
[doodoo] Server Enviroment: development  
[doodoo] Server Startup Time: 212ms  
[doodoo] Server Current Time: 2018-08-21 11:17:19  
[doodoo] Server Running At: http://127.0.0.1:3000
```

**HTTP 请求日志**

```
复制代码<-- GET /demo/index/index  
--> GET /demo/index/index 200 4ms
```

doodoo.js官方文档：https://doodooke.github.io/doodoo.js/#/
【案例】多多客小程序官网：doodooke.com