# 微信小程序 collection.watch实现对云端数据的实时监控

原创TR30 最后发布于2019-09-04 00:10:35 阅读数 388  收藏
展开


    最近在做的小程序项目中，为了实现一个聊天功能的实时性，需要对云端储存的聊天内容进行监控。这篇博文简单的介绍一下，希望对各位朋友有所帮助。

    博文主要内容导引：

            1.介绍利用collection.watch来实现云端数据的监控过程，

            2.对于可能遇到的BUG(db.collection(...).where(...).watch is not a function)提出解决方法。

一、利用collection.watch来实现云端数据的监控

    参考文档：collection.watch介绍文档  And  实时数据推送

    首先说明一下，本文中介绍的watch是监控云端数据库中的数据，并不是对小程序.js文件中的data数据进行监控的。

    下面就是watch的具体使用方法了。在需要实现监控逻辑的.js文件(例如chat.js)中的onLoad函数中添加如下代码：

  // 生命周期函数--监听页面加载
  onLoad: function (options) {
    const db = wx.cloud.database()
    db.collection('Messages').where({
      name: '老王' //这里通过名字找到Messages数据集合中叫“老王”的那一条数据，也即为要监控的数据
    }).watch({
      onChange: function (snapshot) {
        //监控数据发生变化时触发
        console.log('docs\'s changed events', snapshot.docChanges)
        console.log('query result snapshot after the event', snapshot.docs)
        console.log('is init data', snapshot.type === 'init')
      },
      onError:(err) => {
        console.error(err)
      }
    })
  },
    对于我要实现的聊天记录检测功能，我可以将聊天者的名字，以及聊天记录存在云端的Messages数据集合中的一个object数据中，通过上述的代码，我通过筛选名叫“老王”的聊天者，从而定位了这个object数据，再通过watch方法检测这个数据的变化。

    当云端的这个object数据发生变化时，也即和老王聊天的人给老王发送了一条消息，这条消息存在了object数据中，此时watch的OnChange函数会被触发，函数会返回sanpshot参数，此参数中会包含变化后的object数据的内容，由此可以更新老王这一端的聊天数据，从而实现实时对话功能。

二、BUG(db.collection(...).where(...).watch is not a function)的解决

    当你直接把上面的代码粘贴到你的程序中，可能会出现如下的错误。



此时并不是因为代码的逻辑问题，而是collection.watch的使用对于基础的版本是有要求的，是2.8.1。



    在微信开发者工具中进行修改就可以了。修改的方法：设置→项目设置→本地设置→调试基础库，选择2.8.1及其以上的版本即可。



    别忘了在微信公众平台也要修改最低基础库版本要求，这样你的小程序使用者在手机上才能正常调用watch。




