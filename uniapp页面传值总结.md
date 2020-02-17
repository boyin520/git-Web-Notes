# uniapp页面传值总结

直接传一个数字

uni.navigateTo({        url:'../../pages/page/pay?number=1'})

传一个变量

uni.navigateTo({    url:'../../pages/page/pay?title=' this.title})

传的值过长时

uni.navigateTo({     url:'../../pages/page/pay?list=' + encodeURIComponent(JSON.stringify(this.getList))})

传两个及以上变量

uni.navigateTo({url:"../../pages/page/pay?list="+encodeURIComponent(JSON.stringify(this.getList))+ '&allprice=' +this.getAllPrice})

pay页面接收时

onLoad(e){        this.number = e.number        this.title = JSON.parse(e.title)        this.list = JSON.parse(decodeURIComponent(e.list))        }

```
uniapp页面传值总结
直接传一个数字
uni.navigateTo({
        url:'../../pages/page/pay?number=1'
})

传一个变量
uni.navigateTo({
    url:'../../pages/page/pay?title=' this.title
})

传的值过长时
uni.navigateTo({
     url:'../../pages/page/pay?list=' + encodeURIComponent(JSON.stringify(this.getList))
})

传两个及以上变量
uni.navigateTo({
url:"../../pages/page/pay?list="+encodeURIComponent(JSON.stringify(this.getList))+ '&allprice=' +this.getAllPrice
})

pay页面接收时
onLoad(e){
        this.number = e.number
        this.title = JSON.parse(e.title)
        this.list = JSON.parse(decodeURIComponent(e.list))
        }


```

