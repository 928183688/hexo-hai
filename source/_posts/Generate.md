---
title: 小程序canvas合并生成二维码和海报图
cover: https://upload.dianshangdanao.com/202001101224073528.jpg
---

## html结构
```html
<canvas canvas-id="canvas"></canvas>
```
## base64转换为图片方法
```js
const base64src = (base64data, cb) => {
  const [, format, bodyData] = /data:image\/(\w+);base64,(.*)/.exec(base64data) || [];
  if (!format) {
    return (new Error('ERROR_BASE64SRC_PARSE'));
  }
  const filePath = `${wx.env.USER_DATA_PATH}/${FILE_BASE_NAME}.${format}`;
  const buffer = wx.base64ToArrayBuffer(bodyData);
  fsm.writeFile({
    filePath,
    data: buffer,
    encoding: 'binary',
    success() {
      cb(filePath);
    },
    fail() {
      return (new Error('ERROR_BASE64SRC_WRITE'));
    },
  });
};
```
## 生成并且合并海报图js结构
```js
    wx.request({
      // 获取access_token认证
      url: 'https://api.weixin.qq.com/cgi-bin/token?grant_type=client_credential',
      data: {
        appid: // 小程序appid
        secret:// 小程序秘钥
      },
      success: (res) => {
        // 获取二维码
        wx.request({
          url: 'https://api.weixin.qq.com/wxa/getwxacodeunlimit?access_token=' + res.data.access_token,
          method: 'POST',
          data: {
            'path': //要跳转的页面,
            'scene': //你要带的二维码参数,
            "width": //二维码宽度
          },
          responseType: 'arraybuffer', //注意 arraybuffer是以数组的语法处理二进制数据，称为二进制数组。
          success: (res) => {
            //小程序方法解析arraybuffer
            let data = wx.arrayBufferToBase64(res.data);
            //拼接成base64格式
            this.setData({
              imgData: 'data:image/png;base64,' + data
            })
            //使用引入的方法把base64转换为图片
            base64src(this.data.imgData, res => {
              this.setData({
                shareImg: res
              })
            })
            //创建canvas绑定ID
            const ctx = wx.createCanvasContext('canvas')
            wx.getImageInfo({
              src: //海报图路径 ,
              success: res => {
                //绘制海报图
                ctx.drawImage(res.path, 0, 0, 375, 550)
                wx.getImageInfo({
                  src://二维码路径,
                  success: res => {
                    //绘制二维码
                    ctx.drawImage(res.path, 210, 450, 80, 80)
                    //画在版上
                    ctx.draw()
                  }
                })
              }
            })
          }
        })
      }
    })
```
## 一键保存canvas结构图片
```js
  saveCanvasImg() {
    wx.showLoading({
      title: '正在保存',
      mask: true,
    })
    //找canvas的API
    wx.canvasToTempFilePath({
      width: //自定义宽度,
      height: //自定义高度,
      canvasId: //canvasID,
      success: (res) => {
        wx.hideLoading();
        //转换为图片
        const tempFilePath = res.tempFilePath;
        //保存图片API
        wx.saveImageToPhotosAlbum({
          filePath: tempFilePath,
          success(res) {
            wx.showToast({
              title: '图片保存成功',
              duration: 2000
            })
          }
        })
      }
    });
  },
```
