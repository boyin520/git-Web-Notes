# uni-app开发之微信小程序合成自定义分享图并保存

之前说了生成二维码的方法，这里把合成自定义图片的方法也说下，刚好做完这一块功能。

##### 遇到的难点

- 对于canvas来说，所有的布局都需要精准的定位后再绘制出来，所以就要精确定位，还好公司的设计图标注很详细。
- 自定义图片中包括文字、二维码及网络图片。文字和二维码都可以解决，但是想显示网络图片必须先获取图片信息或将图片下载后才能直接使用ctx.drawImage绘制图片。

##### 一步步试探

1. 首先生成二维码，生成后进行回调生成图片
2. 因为有网络图片，所有要用到方法： [uni.getImageInfo(OBJECT)](https://links.jianshu.com/go?to=https%3A%2F%2Funiapp.dcloud.io%2Fapi%2Fmedia%2Fimage%3Fid%3Dgetimageinfo)

```
uni.getImageInfo({
    src: 'https://qiniu.....com/......png',
    success: function (res) {
        console.log(res)
    }
})
```

然后在控制台看到：

![img](https://upload-images.jianshu.io/upload_images/14957158-d893f0b67bfdca28.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

但是出现报错.png

根据文档，在小程序后台-设置-开发设置-服务器域名来进行操作（操作完成后，要等一会再试就生效了）：

![img](https://upload-images.jianshu.io/upload_images/14957158-0de8e84ad1a79592.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

把图片的域名加到downloadFile合法域名中.png

1. 在uni.getImageInfo的成功回调后， [在Canvas上画图](https://links.jianshu.com/go?to=https%3A%2F%2Funiapp.dcloud.io%2Fapi%2Fui%2Fcanvas%3Fid%3D%25e5%259c%25a8canvas%25e4%25b8%258a%25e7%2594%25bb%25e5%259b%25be)，在这里附上绘制页面上居中文字及圆形头像的方法

```
//绘制背景图
ctx.drawImage(res.path,0,0,540,200)
                        
//绘制文本信息
ctx.setFontSize(20);
ctx.setTextAlign('center');
ctx.fillText('活动时间', 270, 540)//270就是x坐标的中心点位置

//头像，原图100*100的尺寸，居中显示直径为100的圆形头像
ctx.save();
ctx.beginPath();
ctx.arc(270, 110, 50, 0, Math.PI * 2, false);
ctx.clip();
ctx.drawImage('/static/img/loading-user.png', 220, 60, 100, 100);
```

1. 绘图生成临时图片

```
setTimeout(function(){
    ctx.draw(false,function(){
        uni.canvasToTempFilePath({
            canvasId: 'shareCanvas',
            success: function(res) {
                uni.hideLoading()
                self.filePath = res.tempFilePath
                callback&&callback(res.tempFilePath)
            },
            fail:function () {
                //TODO
            }
        })
    })
},500)
```

1. 至此，基本就完成啦，调取[uni.saveImageToPhotosAlbum(OBJECT)](https://links.jianshu.com/go?to=https%3A%2F%2Funiapp.dcloud.io%2Fapi%2Fmedia%2Fimage%3Fid%3Dsaveimagetophotosalbum)保存到本地

```
//保存
savePic () {
    let self = this
    uni.showLoading({
        title: '正在保存'
    });
    uni.saveImageToPhotosAlbum({
        filePath: self.filePath,
        success: function () {
            uni.showToast({
                title: '图片保存成功～'
            });
        },
        fail: function (e) {
            //TODO
        },
        complete: function (){
            uni.hideLoading()
        }
    });
}
```

好啦，完成！撒花花～～～