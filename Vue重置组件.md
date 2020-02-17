# Vue重置组件

我是在使用Element-ui的dialog组件弹出表单时遇到的，重复打开时会保留上次的数据及验证。![img](http://hcc-blog.oss-cn-beijing.aliyuncs.com/editor/15519304217214.png)方案一这个方案是我最开始用的，后来找到了更简单的方法![img](http://hcc-blog.oss-cn-beijing.aliyuncs.com/editor/15519304354903.png)
在dialog弹出框关闭事件里利用Vue中的nextTick特性来手动清空表单数据。方案二（推荐）![img](http://hcc-blog.oss-cn-beijing.aliyuncs.com/editor/15519304589272.png)直接在组件上绑定一个动态的key。Vue会拿上次的key来做对比，如果一致则继续复用组件，否则重新渲染`+new Date()`可以获取时间戳![img](http://hcc-blog.oss-cn-beijing.aliyuncs.com/editor/15519304711322.png)是不是很简单？:tw-1f60f: