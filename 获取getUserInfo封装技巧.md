获取getUserInfo封装技巧

	<button open-type="getUserInfo" lang="zh_CN" @getuserinfo="onGetUserInfo" class="cr-sub btn-nobg">用微信号登录</button>
	
	//methods:
		// 获取用户信息
			onGetUserInfo:function(callback) {
			  // 如果本地存储里有，就从本地存储里取
			  let userInfo = uni.getStorageSync('userInfo') ||null
			  
				if (userInfo) {
			    typeof callback ==='function' && callback(userInfo)
					console.log(callback.detail.userInfo)
			  }else {
			    // 如果本地存储里没有，就从微信端获取
			    uni.getUserInfo({
			      lang:'zh_CN',
			      withCredentials:true,
			      success: (res) => {
			        let userInfo = {
			          nickName: res.userInfo.nickName,
			          avatarUrl: res.userInfo.avatarUrl,
			          gender: res.userInfo.gender
			        }
			        uni.setStorageSync('userInfo', userInfo)
			        typeof callback ==='function' && callback(userInfo)
			      },
			      // 如果拒绝授权获取公开信息
			      fail: (res) => {
			        if (res.errMsg.indexOf('auth denied') > -1 || res.errMsg.indexOf('auth deny') > -1) {
			          uni.showModal({
			            title:'提示',
			            content:'为了提供更好的用户体验,系统仅获取您的头像和昵称等基本信息,请在弹出的页面中打开允许使用用户信息',
			            showCancel:false, //不显示弹窗的取消按钮false
			            confirmText:'我知道了',  //弹窗的确认按钮 
			            success: (res) => {
			              if (res.confirm) {
			                uni.openSetting({
			                  success: (res) => {
			                    if (res.authSetting['scope.userInfo'] ===true) {
			                      typeof callback ==='function' && callback(userInfo)									
			                    }
			                  }
			                })
			              }
			            }
			          })
			        }
			      }
			    })
			  }
				
				//写入云数据库
				const db = wx.cloud.database(); //云平台的测试：
				// db.collection('users').add({
				// 	data:{  
				// 		nickName: callback.detail.userInfo.nickName,
				//      _openid: callback.detail.userInfo._openid,
				// 		avatarUrl: callback.detail.userInfo.avatarUrl,
				// 		gender: callback.detail.userInfo.gender,
				// 		UnionID: callback.detail.userInfo.UnionID,
				// 		age: callback.detail.userInfo.age,
				// 		Regdate: callback.detail.userInfo.regdate,
				// 		email: callback.detail.userInfo.email,
				// 		city: callback.detail.userInfo.city,
				// 		phone: callback.detail.userInfo.phone,
				// 		realName: callback.detail.userInfo.realName
				// 	},					
				// }),
				// db.collection('users').get({			
				
				// })				
			},