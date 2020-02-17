# vue new Date() 获取当前时间

**1.new Date() 获取当前时间**

```
  created: function() {
    var aData = new Date();
    console.log(aData) //Wed Aug 21 2019 10:00:58 GMT+0800 (中国标准时间)
    
    this.value =
      aData.getFullYear() + "-" + (aData.getMonth() + 1) + "-" + aData.getDate();
    console.log(this.value) //2019-8-20 
  },
12345678
```

**补充：**
用三元进行单数前面补0

```
  var aData = new Date();
  var month =data.getMonth() < 9 ? "0" + (data.getMonth() + 1) : data.getMonth() + 1;
  var date = data.getDate() <= 9 ? "0" + data.getDate() : data.getDate();
  this.value = data.getFullYear() + "-" + month + "-" + date;
1234
```

在当前时间加一天：86400000 == 24 * 60 * 60 *1000

```
int days = 1;var newDate = new Date(Date.now() + days*24*60*60*1000);
1
```

在main.js添加倒计时一天删除本地数据：

```
// 倒计时一天
Vue.prototype.setExpire = (key, value, expire) => {
  let obj = {
    data: value,
    time: Date.now(),
    expire: expire
  };
  localStorage.setItem(key, JSON.stringify(obj));
}

Vue.prototype.getExpire = key => {
  let val = localStorage.getItem(key);
  if (!val) {
    return val;
  }
  val = JSON.parse(val);
  if ((Date.now() - val.time) > val.expire) {
    // 放清除的数据
    localStorage.removeItem("token");
    localStorage.removeItem(key);
    router.push({ path: "/" });
    return null;
  }
  return val.data;
}
12345678910111213141516171819202122232425
```

其他组件可以调用和获取

```
this.setExpire("token1", "xxxxxxxx", 86400000);
//可以用定时器定时获取当前时间
this.getExpire("token1");
```
------





获取当前的时间

```
nowTime: new Date(), // 当前时间
nowWeek: '星期', //星期几
```

日期格式化

```
filters: {
    formatDate(time) {
      var moment = require("moment");
      return moment(time).format("YYYY-MM-DD");
    }
  },
```

日期转换成星期几

```
var dateArray = nowTime.split("-");
var date = new Date(dateArray[0], parseInt(dateArray[1] - 1), dateArray[2]);
var week = "星期" + "日一二三四五六".charAt(date.getDay());
this.nowWeek = week // 赋值本地数据
alert(week)
```


————————————————
