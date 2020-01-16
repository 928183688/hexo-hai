---
title : 下载图片打包压缩包
cover: https://upload.dianshangdanao.com/201912281847384655.jpg
---
## 打包工具ZIP
```js
https://github.com/nodeca/pako/blob/master/LICENSE
```
## 保存工具FileSaver 
```js
https://github.com/eligrey/FileSaver.js
```
## 转base64
```js
    /**
     * 图片链接转base64
     * @param img
     * @returns {Promise<any>}
     */
    //传入图片路径，返回base64
    getBase64(img) {
        function getBase64Image(img, width, height) {
            var canvas = document.createElement("canvas");
            canvas.width = width ? width : img.width;
            canvas.height = height ? height : img.height;

            var ctx = canvas.getContext("2d");
            ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
            var dataURL = canvas.toDataURL();
            return dataURL;
        }
        var image = new Image();
        image.crossOrigin = 'Anonymous';
        image.src = img;
        var deferred = $.Deferred();
        if (img) {
            image.onload = function () {
                deferred.resolve(getBase64Image(image));
            }
            return deferred.promise();
        }
    },
```
## 转文件对象
```js
    //转文件对象
    getFileResolve(url) {
        return new Promise((resolve, reject) => {
            fetch(url)
                .then(res => {
                    return res.blob()
                })
                .then(blob => {
                    resolve(blob)
                })
                .catch(err => {
                    console.log(err)
                    resolve('error')
                })
        })
    },
```
## 也可利用a标签下载
```js
    /**
     * @param {Fuc} 保存图片
     * @param {String} url 需要下载的文件地址
     * */
    download(url) {
        var oA = document.createElement("a");
        oA.download = ''; // 设置下载的文件名，默认是'下载'
        oA.href = url;
        document.body.appendChild(oA);
        oA.click();
        oA.remove(); // 下载之后把创建的元素删除
    },
```
## 打包图片html结构
```html
 <button class="packImg" data-type="1">打包50张图</button>
 <button class="packImg" data-type="2">打包100张图</button>
 <button class="packImg" data-type="3">打包200张图</button>
 <button class="packImg" data-num="全部">打包全部</button>
```
## 打包图片js
```js
    //打包图片
    let imageSuffix = []; //图片后缀
    $('.packImg').click(function () {
            //图片列表
            const thumbnails = res.data.thumbnails
            if (thumbnails !== '') {
                let zip = new JSZip
                let img = zip.folder("images")
                thumbnails.forEach((v, i) => {
                    let suffix = v.substring(v.lastIndexOf("."))
                    imageSuffix.push(suffix)
                    //转文件对象
                    let arr = download.getFileResolve(v)
                    img.file('图片' + i + imageSuffix[i], arr);
                })
                setTimeout(() => {
                    zip.generateAsync({
                        //接收文件对象
                        type: "blob"
                    }).then(function (content) {
                        // see FileSaver.js
                        saveAs(content, "图片.zip");
                    });
                }, 100)
            } else {
                alert('打包图片失败')
            }
    })
```
