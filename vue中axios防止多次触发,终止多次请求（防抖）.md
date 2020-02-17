# [vue中axios防止多次触发,终止多次请求（防抖）](https://www.cnblogs.com/supredreamer/p/11627075.html)

**需求**

**例如在搜索框中，并不是你输入一个字就需要渲染一次数据，而是取最后一次的输入内容进行搜索。**

**连续按下 AAAAA ，只取最后一次按下时搜索框的内容（即：AAAAA）进行搜索。 而不是搜索跟 A（第一次按下），AA（第二次按下），AAA相关联的内容**

 

**代码：**

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
&lt;script&gt;
import axios from 'axios'
import qs from 'qs'

export default {
    methods: {
        request(keyword) {
            var CancelToken = axios.CancelToken
            var source = CancelToken.source()
              
            // 取消上一次请求
            this.cancelRequest();
            
            axios.post(url, qs.stringify({kw:keyword}), {
                headers: {
                    'Content-Type': 'application/x-www-form-urlencoded',
                    'Accept': 'application/json'
                },
                cancelToken: new axios.CancelToken(function executor(c) {
                    that.source = c;
                })
            }).then((res) =&gt; {
                // 在这里处理得到的数据
                ...
            }).catch((err) =&gt; {
                if (axios.isCancel(err)) {
                    console.log('Rquest canceled', err.message); //请求如果被取消，这里是返回取消的message
                } else {
                    //handle error
                    console.log(err);
                }
            })
        },
        cancelRequest(){
            if(typeof this.source ==='function'){
                this.source('终止请求')
            }
        },
    }
}
&lt;/script&gt;
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 **其他做法：**

 

   **可以使用** **clearTimeout()   setTimeout()  截取，设置一定时常请求一次**