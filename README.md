share_1文件夹是微信内不可用，但是在其他浏览器，可以直接唤起qq，无需登陆

```js
	npm install nativeshare --save  //分享插件
	npm install m-share --save         //分享插件
	npm install qrcode --save          //微信二维码生成
```

share_2文件夹都可用，但是需要qq分享需要输入账号密码

可以根据项目需求，通过以下代码判断：如果在微信浏览器，用文件二，否则用文件一

```js
function isWeiXin(){ 
	var ua = window.navigator.userAgent.toLowerCase(); 
    if(ua.match(/MicroMessenger/i) == 'micromessenger'){ 
        return true; 
    }else{ 
        return false; 
    } 
} 
```

share_3文件夹，share1 和  share2 组合版