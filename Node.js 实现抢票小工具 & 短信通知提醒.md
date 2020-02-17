# Node.js 实现抢票小工具 & 短信通知提醒

# 写在前言

要知道在深圳上班是非常痛苦的事情，特别是我上班的科兴科技园这一块，去的人非常多，每天上班跟春运一样，如果我能换到以前的大冲上班那就幸福了，可惜，换不得。

尤其是我这个站等车的多的一笔，上班公交挤的不行，车满的时候只有少部分人能硬挤上去。通常我只会用两个字来形容这种人：“公交怪”

想当年我朋友瘦的像只猴还能上去，老子身高182体重72kg挤个公交，不成问题，反手一个阻挡，闷声发大财，前面的阿姨你快点阿姨，别磨磨唧唧的，快上去啊阿姨，嗯？你还想挤掉我？你能挤掉我？你能挤掉我！我当场！把车吃了！

....

咳咳，挤公交是不可能挤公交滴，因为今天我发现了一个可以定制路线的网约巴士公众号【深圳xxx】

但是呢，票经常会被抢光，同时我还我发现，有时候会有人退票，这时候就有空余票了，关键是我不可能时时都在公众号上盯着，于是，我就写了一个抢票+短信通知的小工具

# 获取接口信息

## 查看页面结构

这个就是订票页面，显示当前月的车票情况，根据图示，红色为已满，绿色为已购，灰色为不可选

![img](https://user-gold-cdn.xitu.io/2019/10/12/16dbb8fe1d3f8fc3?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

![img](https://user-gold-cdn.xitu.io/2019/10/17/16dd8cd6b2201199?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

1. 定时抓取返回的接口信息
2. 根据接口返回值判断是否有余票

好，审查下源代码看下接口信息，等等，微信浏览器没办法审查源代码，于是

## 使用chrome 调试微信公众号网页页面

首先面临个问题，如果直接copy公众号网页Url在chrome打开的话，就会显示这个画面，他被302重定向到了这个页面，所以是行不通的，只有获取OAuth2.0授权才能进去

![img](https://user-gold-cdn.xitu.io/2019/10/18/16ddaee758b42b4e?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

所以我们得先通过抓包工具，知道手机访问微信公众号网页的时候，需要带什么信息过去，这时候我们就得借助抓包工具，因为我电脑是Mac，用不了`Fiddler`，我用的是`Charles`花瓶，就是下面这位仁兄

![img](https://user-gold-cdn.xitu.io/2019/10/12/16dbba130282d533?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

1. **获取本机IP地址和端口**
2. **设置代理手机上网**
3. 依次执行上面两步

### 获取本机IP地址和端口

第一步，找到端口号，一般默认是8088，但是为了确认可以打开`Proxy`/`Proxy Setting`看下，哦原来我之前设置成了8888

![img](https://user-gold-cdn.xitu.io/2019/10/12/16dbbb6d83d75fc2?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

```
Charles
```

```
help
```

```
Local IP Address
```

![img](https://user-gold-cdn.xitu.io/2019/10/18/16dde2ebaec1fcae?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 设置代理手机上网

首先保证手机跟电脑连接的是同一个wifi，然后在wifi设置那里会有设置代理信息，比如我的猴米...不对，小米9手机！设置如下：

输入上一步获取主机名，端口号就ok了

![img](https://user-gold-cdn.xitu.io/2019/10/12/16dbbb54704d737e?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

输入完成，点击确定后。`Charles`就会弹出一个对话框，问你是否同意接入代理，点击确定allow就行了。

### 用手机访问目标网页

我们用手机访问微信公众号【深圳x出行】进入到抢票页面后，发现`Charles`已经成功抓包到了网页信息，当我们进入这个抢票页面的时候，他会发起两个请求，一个是获取document文档内容，一个post请求获取票务信息。

仔细分析了下，大概明白了业务逻辑：

整个项目技术站是java+jsp，传统写法，用户身份验证主要是cookie+session方案，前端这一块主要是使用`jQuery`。

当用户进入页面的时候，会携带查询参数，如起始站点，时间，车次等信息和cookie请求document文档， 也就是圈起来的这一块，

![img](https://user-gold-cdn.xitu.io/2019/10/18/16dde3053bbff4f2?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

![img](https://user-gold-cdn.xitu.io/2019/10/18/16ddaf459b557988?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

因为还要在请求一次

第二次请求，携带cookie和以上的查询参数发起一个post请求，获取当月的车票信息，也就是日历表内容

下面这个是请求当月票务信息，然而发现他返回的是一堆html节点

好吧...估计是获取到之后直接`append`到`div`里面的，然后渲染生成日历表内容

![img](https://user-gold-cdn.xitu.io/2019/10/12/16dbde3d055047f3?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

接着在手机上操作，选择两个日期，然后点击下单，发送购票请求，拉取购票接口，我们看下购票接口的请求和返回内容：

![img](https://user-gold-cdn.xitu.io/2019/10/17/16dd8d12313f69f7?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

在看看返回的内容：返回一个json字符串数据，里面大概涵盖了下单的成功返回码，时间，id号等等信息

![img](https://user-gold-cdn.xitu.io/2019/10/17/16dd8d1bc2cb32ba?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 记录所需要的信息内容

根据上面的分析，总结下内容： 整个项目用户身份验证是使用`cookie`和`session`方案，请求数据用的是`form data`方式，请求字段啥的我们也都清楚，唯独有一点，就是请求余票的时候，返回的是html节点代码，而不是我们预期的json数据，这样就有个麻烦，我们没办法一目了然的明白他余票的时候是如何显示的

所以我们只能通过`chrome`进行调试，才能得出他是如何判断余票的。

我们找个记事本，记录下信息，记录的内容有：

1. **请求余票接口和购票接口的url地址**
2. **cookie信息**
3. **各自的request参数字段**
4. **user-Agent信息**
5. **各自的response返回内容**

### 设置chrome

有以上信息后，我们就可以开始用chrome调试了， 首先打开`More tools`/`Network conditions`

![img](https://user-gold-cdn.xitu.io/2019/10/12/16dbe019d9130e8f?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

```
user-Agent
```

```
Custom
```

![img](https://user-gold-cdn.xitu.io/2019/10/12/16dbe02119166380?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### Charles抓包本地请求

因为我们要把获取到的cookie填入到chrome里面，以我们的用户身份去访问网页，所以我们需要在请求目标地址的时候，改包修改cookie

首先我们需要开启 `macOS Proxy`，抓包我们的http请求

![img](https://user-gold-cdn.xitu.io/2019/10/12/16dbe201ca6bd22e?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

```
Charles
```

![img](https://user-gold-cdn.xitu.io/2019/10/12/16dbeb7f0e72b6e4?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

```
break points
```

```
sessionId
```

```
cookie
```

```
execute
```

![img](https://user-gold-cdn.xitu.io/2019/10/18/16dde356cd8baabc?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

![img](https://user-gold-cdn.xitu.io/2019/10/12/16dbed1e1521cab4?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

```
jQuery
```

```
CSS
```

审查了下元素发现：

1. **小方块的结构为：**

```
<td class="b">
<span>这里为日期</span>
<span>如果有余票则显示余票数量</span>
</td>
复制代码
```

1. **td的样式名为a代表不可选**
2. **样式名为e代表已满**
3. **样式名为d代表已购**
4. **样式名为b则是我们要找的，代表可选，也就是有余票**

到这一步，整个购票流程就清楚了

到时候我们通过Node.js请求的时候，处理返回数据，用正则去判断是否有余票的class名`b` ，有余票的话，在获取div里面的余票数量内容就Ok了

# Node.js 请求目标接口

## 分析需要开发的功能点

写代码之前我们需要想好功能点，我们需要什么功能:

1. **请求余票接口**
2. **定时请求任务**
3. **有余票则自动请求购票接口下订单**
4. **调用腾讯云短信api接口发送短信通知**
5. **多个用户抢票功能**
6. **抢某个日期的票**

首先`mkdir ticket` 创建名为ticket的文件夹，接着`cd ticket`进入文件夹`npm init`一路瞎几把回车也无妨。 下面开始安装依赖，根据上面的功能需求，我们大概需要：

1. 请求工具，这里看个人习惯，你也可以使用原生的`http.request`，我这里选择用的是`axios`，毕竟`axios`在node端底层也是调用`http.request`

```
cnpm install axios --save
复制代码
```

1. 定时任务 `node-schedule`

```
cnpm install node-schedule --save
复制代码
```

1. node端选择dom节点工具 `cheerio`

```
cnpm install cheerio --save
复制代码
```

1. 腾讯发短信的依赖包 `qcloudsms_js`

```
cnpm install qcloudsms_js 
复制代码
```

1. 热更新包，诺豆的妈妈，`nodemom` （其实不用也可以）

```
cnpm install nodemom --save-dev
复制代码
```

## 开发请求余票接口

接着`touch index.js`创建核心js文件，开始编码：

首先引入所有依赖

```
const axios = require('axios')
const querystring = require("querystring"); //序列化对象，用qs也行，都一样
var QcloudSms = require("qcloudsms_js");
var cheerio = require('cheerio');
var schedule = require('node-schedule');

复制代码
```

然后我们先定义请求参数,来一个obj

```
var obj = {
  data: {
    lineId: 111130, //路线id
    vehTime: 0722, //发车时间，
    startTime: 0751, //预计上车时间
    onStationId: 564492, //预定的站点id
    offStationId: 17990,//到站id
    onStationName: '宝安交通运输局③',  //预定的站点名称
    offStationName: "深港产学研基地",//预定到站名称
    tradePrice: 0,//总金额
    saleDates: '17',//车票日期
    beginDate: '',//订票时间，滞空，用于抓取到余票后填入数据
  },
  phoneNumber: 123123123, //用户手机号，接收短信的手机号
  cookie: 'JSESSIONID=TESTCOOKIE', // 抓取到的cookie
  day: "17" //定17号的票，这个主要是用于抢指定日期的票，滞空则为抢当月所有余票
}

复制代码
```

接着声明一个名为`queryTicket`的类，为啥要用类呢，因为基于第五个需求点，多个用户抢票的时候，我们分别`new`一下就行了，

同时我们希望能够记录请求余票的次数，和当抢到票后自动停止查询余票得操作，所以给他加上个计数变量`times`和是否停止的变量，布尔值`stop`

编写代码:

```
class QueryTicket{
  /**
   *Creates an instance of QueryTicket.
   * @param {Object} { data, phoneNumber, cookie, day }
   * @param data {Object} 请求余票接口的requery参数
   * @param phoneNumber {Number} 用户手机号，短信需要用到
   * @param cookie {String} cookie信息
   * @params day {String} 某日的票，如'18'
   * @memberof QueryTicket 请求余票接口
   */
  constructor({ data, phoneNumber, cookie, day }) {
    this.data = data 
    this.cookie = cookie
    this.day = day
    this.phoneNumber = phoneNumber
    this.postData = querystring.stringify(data)
    this.times = 0;   //记录次数
    var stop = false //通过特定接口才能修改stop值，防止外部随意串改
    this.getStop = function () { //获取是否停止
      return stop 
    }
    this.setStop = function (ifStop) { //设置是否停止
      stop = ifStop
    }
  }
}

复制代码
```

下面开始定义原型方法，为了方便维护，我们把逻辑拆分成各个函数

```
class QueryTicket{
  constructor({ data, phoneNumber, cookie, day }) {
  //constructor代码... 
  }
    init(){}//初始化
    handleQueryTicket(){}//查询余票的逻辑
    requestTicket(){} //调用查询余票接口
    handleBuyTicket(){} //购票相关逻辑
    requestOrder(){}//调用购票接口
    handleInfoUser(){}//通知用户的逻辑
    sendMSg(){} //发短信接口
}

复制代码
```

所有数据都是基于查询余票的操作，因此我们先开发这部分功能

```
class QueryTicket{
  constructor({ data, phoneNumber, cookie, day }) {
  //constructor代码... 
  }
  //初始化,因为涉及到异步请求，所以我们使用`async await`
   async init(){
          let ticketList = await this.handleQueryTicket() //返回查询到的余票数组
    }
    //查询余票的逻辑
    handleQueryTicket(){ 
    let ticketList = [] //余票数组
    let res = await this.requestTicket()
    this.times++ //计数器，记录请求查询多少次
    var str = res.data.replace(/\\/g, "") //格式化返回值
    var $ = cheerio.load(`<div class="main">${str}</div>`) // cheerio载入查询接口response的html节点数据
    let list = $(".main").find(".b") //查找是否有余票的dom节点
    // 如果没有余票，打印出请求多少次,然后返回，不执行下面的代码
    if (!list.length) {
      console.log(`用户${this.phoneNumber}：无票，已进行${this.times}次`)
      return
    }

    // 如果有余票
    list.each((idx, item) => {
      var str = $(item).html() //str这时格式是<span>21</span><span>&$x4F59;0</span>
      //最后一个span 的内容其实"余0"，也就是无票，只不过是被转码了而已
      //因此要在下一步对其进行格式化
      var arr = str.split(/<span>|<\/span>|\&\#x4F59\;/).filter(item => !!item === true) 
      let data = {
        day: arr[0],
        ticketLeft: arr[1]
      }
      
      //如果是要抢指定日期的票
      if (this.day) {
      //如果有指定日期的余票
        if (parseInt(data.day) === parseInt(data.day)) {
          ticketList.push(data)
        }
      } else {
      //如果不是，则返回查询到的所有余票
        ticketList.push(data)
      }
    })
    return ticketList
    }
     //调用查询余票接口
    requestTicket(){
    return axios.post('http://weixin.xxxx.net/ebus/front/wxQueryController.do?BcTicketCalendar', this.postData, {
      headers: {
        'Content-Type': 'application/x-www-form-urlencoded',
        'User-Agent': "Mozilla/5.0 (iPhone; CPU iPhone OS 8_0 like Mac OS X) AppleWebKit/600.1.4 (KHTML, like Gecko) Mobile/12A365 MicroMessenger/5.4.1 NetType/WIFI",
        "Cookie": this.cookie
      }
    })   
    }
    handleBuyTicket(){} //购票相关逻辑
    requestOrder(){}//调用购票接口
    handleInfoUser(){}//通知用户的逻辑
    sendMSg(){} //发短信接口
}

复制代码
```

来解释下那行正则，`cheerio`抓取到的dom是长这样的，第一个`span`内容是日期，第二个是余票数量

![img](https://user-gold-cdn.xitu.io/2019/10/18/16ddcebe5ff5a6e7?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

```
ticketList
```

![img](https://user-gold-cdn.xitu.io/2019/10/18/16ddcec5a3ddac0f?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

## 开发购票功能

首先我们在`init`方法里做个判断，如果有余票才去购票，没有余票购个毛

```
class QueryTicket{
  constructor({ data, phoneNumber, cookie, day }) {
  //constructor代码... 
  }
  //初始化
   async init(){
    let ticketList = await this.handleQueryTicket()
    //如果有余票
    if (ticketList.length) {
    //把余票传入购票逻辑方法，返回短信通知所需要的数据
      let resParse = await this.handleBuyTicket(ticketList)
    }
    }
    
    //查询余票的逻辑
   async handleQueryTicket(){
    // 查询余票代码...
    }
    //调用查询余票接口
    requestTicket(){
    //调用查询余票接口代码...    
    } 
    //购票相关逻辑
   async handleBuyTicket(ticketList){
    let year = new Date().getFullYear() //年份，
    let month = new Date().getMonth() + 1 //月份，拼接购票日期用得上，因为余票接口只返回几号
    let {
      onStationName,//起始站点名
      offStationName,//结束站点名
      lineId,//线路id
      vehTime,//发车时间
      startTime,//预计上车时间
      onStationId,//上车的站台id
      offStationId //到站的站台id
      } = this.data // 初始化的数据

    let station = `${onStationName}-${offStationName}` //站点，发短信时候用到:"宝安交通局-深港产学研基地"
    let dateStr = ""; //车票日期
    let tickAmount = "" //总张数
    ticketList.forEach(item => {
      dateStr = dateStr + `${year}-${month}-${item.day},`
      tickAmount = tickAmount + `${item.ticketLeft}张,`
    })

    var buyTicket = {
      lineId,//线路id
      vehTime,//发车时间
      startTime,//预计上车时间
      onStationId,//上车的站点id
      offStationId,//目标站点id
      tradePrice: '5', //金额
      saleDates: dateStr.slice(0, -1),
      payType: '2' //支付方式，微信支付
    }

    // 调用购票接口
     let data = querystring.stringify(buyTicket)
     let res = await this.requestOrder(data) //返回json数据，是否购票成功等等
     //把发短信所需要数据都要传入
    return Object.assign({}, JSON.parse(res.data), { queryParam: { dateStr, tickAmount, startTime, station } })
    }//购票相关逻辑
    //调用购票接口
    requestOrder(obj){
    return axios.post('http://weixin.xxxx.net/ebus/front/wxQueryController.do?BcTicketBuy', obj, {
      headers: {
        'Content-Type': 'application/x-www-form-urlencoded',
        'User-Agent': "Mozilla/5.0 (iPhone; CPU iPhone OS 8_0 like Mac OS X) AppleWebKit/600.1.4 (KHTML, like Gecko) Mobile/12A365 MicroMessenger/5.4.1 NetType/WIFI",
        "Cookie": this.cookie
      }
    })
    }
    handleInfoUser(){}//通知用户的逻辑
    sendMSg(){} //发短信接口
}

复制代码
```

到这里，查询余票，购票这两个核心操作已经完成。

目前还剩下，如何通知用户是否购票成功。

之前我尝试过使用qq邮箱的smtp服务，抢票成功后发送邮件通知，但是我觉得吧，并不好用，主要是我没有打开邮箱的习惯，没网也收不到，所以，并没有采纳这个方案。

加上之前我注册过企业认证的公众号，腾讯云免费送了我1000条短信通知，而且短信也比较直观，所以我这里就安装腾讯云的SDK，部署了一套发短信的功能。

## 腾讯云短信的相关内容

其实看看文档就行了，我也是copy文档，注意看短信单发那部分

[cloud.tencent.com/document/pr…](https://cloud.tencent.com/document/product/382/3772)

如果跟我一样有企业认证的话，看快速入门这里就行了，一步步跟着操作

![img](https://user-gold-cdn.xitu.io/2019/10/18/16dddd7629b40c48?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

```
{Number}
```

就是说短信的模板是固定的，但是里面有`{Number}`的内容可以自定义

调用的时候，里面的数字对应着传过去的参数数组序号，{1}代表数组[0]参数，以此类推

![img](https://user-gold-cdn.xitu.io/2019/10/18/16dddd6c1dd1a411?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

提交审核，审核一般很快就通过，也就是几十万毫秒吧

![img](https://user-gold-cdn.xitu.io/2019/10/18/16dddf02494589bf?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

![img](https://user-gold-cdn.xitu.io/2019/10/18/16dddefa8b62db68?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

## 开发通知功能

```
class QueryTicket{
  constructor({ data, phoneNumber, cookie, day }) {
  //constructor代码... 
  }
  //初始化
   async init(){
    let ticketList = await this.handleQueryTicket()
    //如果有余票
    if (ticketList.length) {
    //把余票传入购票逻辑方法，返回短信通知所需要的数据
      let resParse = await this.handleBuyTicket(ticketList)
    //执行通知逻辑
     this.handleInfoUser(resParse)
    }
    }
    
    //查询余票的逻辑
   async handleQueryTicket(){
    // 查询余票代码...
    }
    //调用查询余票接口
    requestTicket(){
    //调用查询余票接口代码...    
    } 
    //购票相关逻辑
   async handleBuyTicket(ticketList){
    //购票代码...
    }
    //调用购票接口
    requestOrder(obj){
    //购票接口请求代码...
    }
    //通知用户的逻辑
    async handleInfoUser(parseData){
    //获取上一步购票的response数据和我们拼接的数据
    let { returnCode, returnData: { main: { lineName, tradePrice } }, queryParam: { dateStr, tickAmount, startTime, station } } = parseData
    //如果购票成功，则返回500
    if (returnCode === "500") {
      let res = await this.sendMsg({
        dateStr, //日期
        tickAmount: tickAmount.slice(0, -1), //总张数
        station, //站点
        lineName, //巴士名称/路线名称
        tradePrice,//总价
        startTime,//出发时间
        phoneNumber: this.phoneNumber,//手机号
      })
      //如果发信成功，则不再进行抢票操作
      if (res.result === 0 && res.errmsg === "OK") {
        this.setStop(true)
      } else {
      //失败不做任何操作
        console.log(res.errmsg)
      }
    } else {
      //失败不做任何操作
      console.log(resParse['returnInfo'])
    }        
    }
    //发短信接口
    sendMSg(){
    let { dateStr, tickAmount, station, lineName, phoneNumber, startTime, tradePrice } = obj
    var appid = 140034324;  // SDK AppID 以1400开头
    // 短信应用 SDK AppKey
    var appkey = "asdfdsvajwienin23493nadsnzxc";
    // 短信模板 ID，需要在短信控制台中申请
    var templateId = 7839;  // NOTE: 这里的模板ID`7839`只是示例，真实的模板 ID 需要在短信控制台中申请
    // 签名
    var smsSign = "测试短信";  // NOTE: 签名参数使用的是`签名内容`，而不是`签名ID`。这里的签名"腾讯云"只是示例，真实的签名需要在短信控制台申请
    // 实例化 QcloudSms
    var qcloudsms = QcloudSms(appid, appkey);
    var ssender = qcloudsms.SmsSingleSender();
    // 这里的params就是短信里面可以自定义的内容，也就是填入{1}{2}..的内容
    var params = [dateStr, station, lineName, startTime, tickAmount, tradePrice];
    //用promise来封装下异步操作
    return new Promise((resolve, reject) => {
      ssender.sendWithParam(86, phoneNumber, templateId, params, smsSign, "", "", function (err, res, resData) {
        if (err) {
          reject(err)
        } else {
          resolve(resData)
        }
      });
    })
    } 
}
复制代码
```

如果发信成功，返回`result:0`

![img](https://user-gold-cdn.xitu.io/2019/10/18/16dddf647e452e11?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

到这里，大部分需求已经完成了，还剩下一个定时任务

## 定时任务

也声明一个类，这里我们用到的是`schedule`

```
// 定时任务
class SetInter {
  constructor({ timer, fn }) {
    this.timer = timer // 每几秒执行
    this.fn = fn //执行的回调
    this.rule = new schedule.RecurrenceRule(); //实例化一个对象
    this.rule.second = this.setRule() // 调用原型方法，schedule的语法而已
    this.init()
  }
  setRule() {
    let rule = [];
    let i = 1;
    while (i < 60) {
      rule.push(i)
      i += this.timer
    }
    return rule //假设传入的timer为5，则表示定时任务每5秒执行一次
    // [1, 6, 11, 16, 21, 26, 31, 36, 41, 46, 51, 56] 
  }
  init() {
    schedule.scheduleJob(this.rule, () => {
      this.fn() // 定时调用传入的回调方法
    });
  }
}

复制代码
```

## 多个用户抢票

假设我们有两个用户要抢票，所以定义两个obj，实例化下`QueryTicket`类

```
  data: { //用户1
    lineId: 111130,
    vehTime: 0722,
    startTime: 0751,
    onStationId: 564492,
    offStationId: 17990,
    onStationName: '宝安交通运输局③',
    offStationName: "深港产学研基地",
    tradePrice: 0,
    saleDates: '',
    beginDate: '',
  },
  phoneNumber: 123123123,
  cookie: 'JSESSIONID=TESTCOOKIE',
  day: "17"
}
var obj2 = { //用户2
  data: {
    lineId: 134423,
    vehTime: 1820,
    startTime: 1855,
    onStationId: 4322,
    offStationId: 53231,
    onStationName: '百度国际大厦',
    offStationName: "裕安路口",
    tradePrice: 0,
    saleDates: '',
    beginDate: '',
  },
  phoneNumber: 175932123124,
  cookie: 'JSESSIONID=TESTCOOKIE',
  day: "" 
}
var ticket = new QueryTicket(obj) //用户1
var ticket2 = new QueryTicket(obj2) //用户2

new SetInter({
  timer: 1, //每秒执行一次，建议5秒，不然怕被ip拉黑，我这里只是为了方便下面截图
  fn: function () {
    [ticket,ticket2].map(item => { //同时进行两个用户的抢票
      if (!item.getStop()) {  //调用实例的原型方法，判断是否停止抢票，如果没有则继续抢
        item.init()
      } else { // 如果抢到票了，则不继续抢票
        console.log('stop')
      }
    })
  }
})

复制代码
```

`node index.js` 运行下，跑起来了

![img](https://user-gold-cdn.xitu.io/2019/10/21/16dedde47ecd677b?imageslim)

![img](https://user-gold-cdn.xitu.io/2019/10/18/16dde1daec3f1eab?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

![img](https://user-gold-cdn.xitu.io/2019/10/18/16ddf278e51019c6?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

# 写在最后

其实可以在此基础上还能添加更多功能，比如直接抓取登录接口获取cookie，指定路线抢票，还有错误处理啊啥的

因为这个只是我个人使用，所以只有这点功能就足够了。

如果想把它做成一个完整的项目，建议使用ts加持 ，关于ts我推荐阅读这篇JD前端写的文章

[juejin.im/post/5d8efe…](https://juejin.im/post/5d8efeace51d45782b0c1bd6)