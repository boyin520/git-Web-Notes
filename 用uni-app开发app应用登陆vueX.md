# 用uni-app开发app应用登陆

## 后端程序

我使用的是laravel框架作为app的接口,前后端分离就涉及到登陆这个问题了,uni-app不支持读写cookie,所以不能使用cookie来保存登陆状态.后端我使用的是基于OAuth的扩展包Passport扩展包,这里就不细写安装这个扩展包的过程了.详情可以参考[学院君的文章](https://link.juejin.im/?target=https%3A%2F%2Flaravelacademy.org%2Fpost%2F9745.html).

## uni-app发送登陆请求

默认的uni-app是一个使用 Vue.js.竟然这里登陆状态没有cookie了,那么我们咋么保存登陆状态呢,我们是通过vuex存储登陆状态的.

```
const store = new Vuex.Store({
    state: {
        /**
         * 是否需要强制登录
         */
        forcedLogin: false,
        hasLogin: false,
        userInfo:{},
    },
    mutations: {
        login(state, provider) {
			state.hasLogin = true;
            state.userInfo.access_token = provider.access_token,  //access_token 是授权令牌,接下来访问后端的接口都带上token
            state.userInfo.token_type = provider.token_type,      //token_type 表示认证类型是 Bearer,
			                                                      //我们可以将这个 access_token 值设置到 Bearer Authentication 请求头去请求需要认证的后端 API 接口
			uni.setStorage({
				key:'userInfo',
				data:provider,
			})
        },
        logout(state) {
            state.hasLogin = false;
			state.userinfo={};
			uni.removeStorage({
				key:'userInfo'
			})
        }
    }
})
 
export default store
```

## 登陆页面

```
bindLogin() {				
    /**
     * 客户端对账号信息进行一些必要的校验。
     * 实际开发中，根据业务需要进行处理，这里仅做示例。
     */
    if (this.account.length < 5) {
        uni.showToast({
            icon: 'none',
            title: '账号最短为 5 个字符'
        });
        return;
    }
    if (this.password.length < 6) {
        uni.showToast({
            icon: 'none',
            title: '密码最短为 6 个字符'
        });
        return;
    }
/**
 * 下面简单模拟下服务端的处理
 * 检测用户账号密码是否在已注册的用户列表中
 * 实际开发中，使用 uni.request 将账号信息发送至服务端，客户端在回调函数中获取结果信息。
 */
  const data = {
			grant_type:'password',
			client_id: 1,   
			client_secret: '数据库中client_secret',//同上
			username: this.account,
			password: this.password,
  };
// const validUser = service.getUsers().some(function (user) {
//   return data.account === user.account && data.password === user.password;
// });
  uni.request({
  	url:'http://guohang.test/oauth/token', //获取访问令牌
		data:
		 {
			grant_type:'password',
			client_id: 1,   //安装passport,完成间授权注册,数据库生成的
			client_secret: '数据库中client_secret',//同上
			username: this.account,
			password: this.password,
		 },
		method:'POST',
		header:{
			'Access-Control-Allow-Origin': '*',  //跨域加上头
			'Content-Type': 'application/json; charset=UTF-8'
		},		
		success:(res)=> {//检验成功返回状态码,访问令牌等参数,使用vuex保存状态,后面请求服务器接口都要带上
			const info={      
				access_token:res.data.access_token,
				token_type:res.data.token_type,
			};		
			if(res.statusCode==200){
				this.login(info);
						 uni.reLaunch({
						 url:'../main/main',
						 });
			}else{
				uni.showToast({
				    icon: 'none',
				    title: '用户账号或密码不正确',
				});
			}					
		}, 
          })
   },...mapMutations(['login']) , 
```

## 认证的页面

因为页面有些是需要登陆之后才能进行操作的,所以我们应该有个拦截器来进行判断是否登陆,没有登陆就跳转登陆页面,

```
import {
        mapState,
		mapMutations
    } from 'vuex'
 
    export default {
        computed: mapState(['forcedLogin', 'hasLogin', 'userName']),
        onLoad() {
            if (!this.hasLogin) {
                uni.showModal({
                    title: '未登录',
                    content: '您未登录，需要登录后才能继续',
                    /**
                     * 如果需要强制登录，不显示取消按钮
                     */
                    showCancel: !this.forcedLogin,
                    success: (res) => {
                        if (res.confirm) {
							/**
							 * 如果需要强制登录，使用reLaunch方式
							 */
                            if (this.forcedLogin) {
                                uni.reLaunch({
                                    url: '../login/login'
                                });
                            } else {
                                uni.navigateTo({
                                    url: '../login/login'
                                });
                            }
                        }
                    }
                });
            }
        }
    }
```

### 这只是一个很小的轮子,给你提供思路,最近自己在用这个做混合开发,所以做个并不是很细的笔记.

转载于:https://juejin.im/post/5c8915db6fb9a04a10302610