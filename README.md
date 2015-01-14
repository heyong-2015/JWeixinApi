微信公众平台JWeixinApi

JWeixinApi的诞生

腾讯官方于2015年1月9日发布了微信的JS-SDK，详细接口可以参考：见[微信JS-SDK说明文档](http://mp.weixin.qq.com/wiki/7/aaa137b55fb2e0456bf8dd9148dd613f.html)
由于微信官方的接口互相独立，但是在事件使用中，我们可能会是很多接口一起使用，所以直接使用官方的接口比较麻烦，重复代码相对较多。为了更方便的使用，我对官方的接口进行了简单的封装，并开源处理，方便大家使用。


快速使用
1、引入js文件
由于该接口是对微信官方js的封装，需要同时引入微信官方的js-sdk文件和JWeixinApi的js文件。引入如下两个文件：
jweixin-1.0.0.js
jweixinapi.js

2、通过config接口注入权限验证配置
该步骤和官方文档一样，详情[微信JS-SDK说明文档](http://mp.weixin.qq.com/wiki/7/aaa137b55fb2e0456bf8dd9148dd613f.html#.E6.AD.A5.E9.AA.A4.E4.BA.8C.EF.BC.9A.E9.80.9A.E8.BF.87config.E6.8E.A5.E5.8F.A3.E6.B3.A8.E5.85.A5.E6.9D.83.E9.99.90.E9.AA.8C.E8.AF.81.E9.85.8D.E7.BD.AE)
```javascript
wx.config({
    debug: true, // 开启调试模式,调用的所有api的返回值会在客户端alert出来，若要查看传入的参数，可以在pc端打开，参数信息会通过log打出，仅在pc端时才会打印。
    appId: '', // 必填，公众号的唯一标识
    timestamp: , // 必填，生成签名的时间戳
    nonceStr: '', // 必填，生成签名的随机串
    signature: '',// 必填，签名，见附录1
    jsApiList: [] // 必填，需要使用的JS接口列表，所有JS接口列表见附录2
});

```
3、通过ready接口处理成功验证
```javascript

wx.ready(function(){

    // config信息验证后会执行ready方法，所有接口调用都必须在config接口获得结果之后，config是一个客户端的异步操作，所以如果需要在页面加载时就调用相关接口，则须把相关接口放在ready函数中调用来确保正确执行。对于用户触发时才调用的接口，则可以直接调用，不需要放在ready函数中。
});
```

4、分享功能
```javascript
wx.ready(function(){
    // 初始化wx对象
    WeixinApi.ready(wx);
	
		// 微信分享的数据
        var wxData = {
            "imgUrl" : "http://www.jiayouxiaogui.cn/images/logo.png",// 分享图标
            "link" : "http://www.jiayouxiaogui.cn", //分享链接
            "desc" : "家有小鬼", //分享描述
            "title" : "家有小鬼" // 分享标题
        };
        
        // 分享的回调函数
        var wxCallbacks={
        	success:function(){
        	  // 用户确认分享后执行的回调函数
        		alert("success");
        	},
        	cancel:function(){
        	  // 用户取消分享后执行的回调函数
        		alert("cancel");
        	},
  		    fail:function(){
  		    	//接口调用失败时执行的回调函数
  		    },
  		    complete:function(){
  		    	// 接口调用完成时执行的回调函数，无论成功或失败都会执行。
  		    },
  		    trigger:function(){
  		    	//监听Menu中的按钮点击时触发的方法，该方法仅支持Menu中的相关接口。 
  		    }
        };
	
		    // 用户点开右上角popup菜单后，点击分享给好友，会执行下面这个代码
        WeixinApi.shareToFriend(wxData, wxCallbacks);

        // 点击分享到朋友圈，会执行下面这个代码
        WeixinApi.shareToTimeline(wxData, wxCallbacks);

        // 点击分享到腾讯微博，会执行下面这个代码
        WeixinApi.shareToWeibo(wxData, wxCallbacks);
        
        //点击分享到QQ，会执行下面这个代码
		    WeixinApi.shareToQQ(wxData, wxCallbacks);
	}
```
5、隐藏右上角option menu入口
```javascript
wx.ready(function(){
  //隐藏右上角的option menu
  WeixinApi.hideOptionMenu();
  //显示
  //WeixinApi.showOptionMenu();
  //批量隐藏功能按钮接口
  //WeixinApi.hideMenuItems();
  //批量显示功能按钮接口
  //WeixinApi.showMenuItems();
  //隐藏所有非基础按钮接口
  //WeixinApi.hideAllNonBaseMenuItem();
  //显示所有功能按钮接口
  //WeixinApi.showAllNonBaseMenuItem();
}
```

6、隐藏底部工具栏

```javascript
wx.ready(function(){
  // 隐藏
	WeixinApi.hideToolbar();
	
	// 显示
	// WeixinApi.showToolbar();
}
```

