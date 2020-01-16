---
title: 一键分享QQ空间 QQ好友
cover: https://upload.dianshangdanao.com/201912281850363761.jpg
---
## 分享工具
```html
https://github.com/overtrue/share.js
```
## html
```html
<div class="share">

</div>
```
## js
```js
	var $config = {
		url: url,
		title: '内容',
		sites: ['qzone', 'qq', 'weibo', 'wechat', 'douban'], // 启用的站点
		wechatQrcodeTitle: "微信扫一扫：分享", // 微信二维码提示文字
		wechatQrcodeHelper: '<p>微信里点“发现”，扫一下</p><p>二维码便可将本文分享至朋友圈。</p>',
		image: 'http://q1mnsxjq8.bkt.clouddn.com/201911301256243962.png'
	};
	socialShare('.share', $config);
```