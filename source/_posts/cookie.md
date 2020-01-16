---
title: Jquey Cookie 一天弹一次窗和不再提醒功能
cover: https://upload.dianshangdanao.com/202001101529539744.jpg
---

## 下载cookie
```html
https://github.com/carhartl/jquery-cookie/blob/master/src/jquery.cookie.js
```
```js
if ($.cookie("isClose") != 'yes') {
     //显示
     show()
     //关闭
     $('').click(function () {
         //隐藏
         hide()
         $.cookie("isClose", 'yes', {
             expires: 1 / 8640
         }); //测试十秒
        $.cookie("isClose",'yes',{
             expires:1
         });//一天    
     });
     //不再提示
     $('').click(function () {
         $.cookie("isClose", 'yes', {
             expires: 100000
         });
     })
 }
```