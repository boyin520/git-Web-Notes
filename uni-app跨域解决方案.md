# [uni-app跨域解决方案](https://segmentfault.com/a/1190000017911280)

 <https://segmentfault.com/u/potato1314>

[github地址，喜欢的可以star下哦](https://github.com/xiaowang1314/uniapp-plugin-collections)

1.官方推荐
[cors和插件安装解决跨域](http://ask.dcloud.net.cn/article/35267)

2.配置uni-app 中 manifest.json->h5->devServer
manifest.json

```
    "h5": {
        "devServer": {
            "port": 8000,
            "disableHostCheck": true,
            "proxy": {
                "/dpc": {
                    "target": "http://dpc.dapeis.net",
                    "changeOrigin": true,
                    "secure": false
                }
            }
        }
    }
```

http请求

```
uni.request({
    url: '/dpc/getUserInfo', 
    success: (res) => {
        console.log(res.data);
    }
});
```

这样请求webpack会解析为请求`http://dpc.dapeis.net/dpc/`