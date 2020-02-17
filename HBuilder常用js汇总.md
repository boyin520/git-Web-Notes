# HBuilder常用js汇总

分类：Native.js

```
复制代码/*  
 * 服务器的地址  
 */  
var strservicef = '';  
var strservices = '';  

/*  
 * 主要的公共函数  
 */  
var Fun_App = {  
    /**  
     * 完整的打开新页面方法  
     * @param {Object} _url  
     */  
    OpenPage: function(id, gethtmlurl,action,sendvalue) {  
        var page = mui.preload({  
            url:gethtmlurl,  
            id: gethtmlurl,  
        });  
        var getid = document.getElementById(id);  
        getid.addEventListener('tap', function() {  
                mui.openWindow({  
                    url:gethtmlurl,  
                    id:gethtmlurl,  
                    styles:{  
                      top:"44px",  
                      bottom:"50px",  
                      width:"100%",  
                      height:"100%"  
                    },  
                    extras:{  
                      kid:sendvalue  
                    },  
                    show:{  
                      autoShow:true,//页面loaded事件发生后自动显示，默认为true  
                      aniShow:action,//页面显示动画，默认为”slide-in-right“；  
                      duration:100//页面动画持续时间，Android平台默认100毫秒，iOS平台默认200毫秒；  
                    },  
                    waiting:{  
                      autoShow:true,//自动显示等待框，默认为true  
                      title:"正在加载..."//等待对话框上显示的提示内容  
                    }  
                })  
        });  
    },  
    /*  
     * 页面间传值的获取  
     */  
    getextrasdata: function(kid){  
        var self = plus.webview.currentWebview();  
        return self.kid;  
    },  
    /*  
     * ajax 数据请求方法  
     */  
    ExAjax: function(url, rdata) {  
        mui.ajax({  
            url: url,  
            type: "post",  
            data: rdata.config,  
            dataType: 'json',  
            timeout:10000,  
            success: function(data) {  
                rdata.fun_Success(data);  
            },  
            error: function(xhr, type, errorThrown) {  
                console.log(JSON.stringify(xhr) + type + "---" + errorThrown);  
            }  
        });  
    },  
    /*  
     * 数据存储的函数  
     */  
    storagedata: function(kname,sdata){  
        localStorage.setItem(kname,sdata);  
    },  
    /*  
     * 数据读取  
     */  
    getdata: function(kname){  
        return localStorage.getItem(kname);  
    },  
    /*  
     * 数据删除  
     */  
    deldata: function(kname){  
        localStorage.removeItem(kname);  
    },  
    /*  
     * 手势配置  
     */  
    gesture: function(){  
        var gs = {  
            gestureConfig:{  
               tap: true, //默认为true  
               doubletap: true, //默认为false  
               longtap: true, //默认为false  
               swipe: true, //默认为true  
               drag: true //默认为true  
        }  
        };  
        return gs;  
    },  
    /*  
     * 隐藏滚动条  
     */  
    delscroll: function(){  
        plus.webview.currentWebview().setStyle({  
            scrollIndicator: 'none',  
        });  
    },  
    /*   
     * 返回键退出程序  
     * 1秒内，连续两次按返回键，则退出应用  
     */  
    FunBackQuitAppL: function(){  
        var backFirst = null;  
        this.QuitApp = function() {  
            //首次按键，提示‘再按一次退出应用’  
            if (!backFirst) {  
                backFirst = new Date().getTime();  
                mui.toast('再按一次退出应用程序');  
                setTimeout(function() {  
                    backFirst = null;  
                }, 1000);  
            } else {  
                if ((new Date()).getTime() - backFirst < 1000) {  
                    plus.runtime.quit();  
                }  
            }  
        }  
    }  
}  
```