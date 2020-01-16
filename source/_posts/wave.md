---
title: css波浪特效&&css球形波浪
cover: https://upload.dianshangdanao.com/202001101231071085.png
---
## 球形波浪
```html
<div class="container">
    <p class="title">快捷</p>
    <span class="wave"></span>
    <span class="wave"></span>
    <span class="wave"></span>
</div>
```
```css
  html,
 body {
     height: 100%;
     display: flex;
     align-items: center;
     justify-content: center;
 } .container {
     width: 300px;
     height: 300px;
     border-radius: 50%;
     box-shadow: 0 2px 30px rgba(0, 0, 0, 0.2);
     position: relative;
     overflow: hidden;
 } .container .title {
     color: white;
     font-size: 24px;
     font-family: serif;
     text-align: center;
     text-transform: uppercase;
     line-height: 250px;
     letter-spacing: 0.4em;
     z-index: 1;
     position: absolute;
     width: 100%;
 } .container .wave {
     width: 500px;
     height: 500px;
     position: absolute;
     background-color: deepskyblue;
     top: -250px;
     left: -100px;
     filter: opacity(0.4);
     border-radius: 43%;
     animation: drift linear infinite;
     transform-origin: 50% 48%;
 } .container .wave:nth-of-type(1) {
     animation-duration: 5s;
 } .container .wave:nth-of-type(2) {
     animation-duration: 7s;
 } .container .wave:nth-of-type(3) {
     animation-duration: 9s;
     background-color: red;
     filter: opacity(0.1);
 } @keyframes drift {
     from {
         transform: rotate(360deg);
     }
 }  
```
## 底部波浪
```html
<div class="waveWrapper waveAnimation">
    <div class="waveWrapperInner bgTop">
        <div class="wave waveTop" style="background-image: url('image/wave-top.png')"></div>
    </div>
    <div class="waveWrapperInner bgMiddle">
        <div class="wave waveMiddle" style="background-image: url('image/wave-mid.png')"></div>
    </div>
    <div class="waveWrapperInner bgBottom">
        <div class="wave waveBottom" style="background-image: url('image/wave-bot.png')"></div>
    </div>
</div>
```
```css
 @keyframes move_wave {
     0% {
         transform: translateX(0) translateZ(0) scaleY(1)
     }     50% {
         transform: translateX(-25%) translateZ(0) scaleY(0.55)
     }     100% {
         transform: translateX(-50%) translateZ(0) scaleY(1)
     }
 } .waveWrapper {
     overflow: hidden;
     position: absolute;
     left: 0;
     right: 0;
     bottom: 0;
     top: 0;
     margin: auto;
 } .waveWrapperInner {
     position: absolute;
     width: 100%;
     overflow: hidden;
     height: 100%;
     bottom: -1px;
 } .bgTop {
     z-index: 15;
     opacity: 0.5;
 } .bgMiddle {
     z-index: 10;
     opacity: 0.75;
 } .bgBottom {
     z-index: 5;
 } .wave {
     position: absolute;
     left: 0;
     width: 200%;
     height: 100%;
     background-repeat: repeat no-repeat;
     background-position: 0 bottom;
     transform-origin: center bottom;
 } .waveTop {
     background-size: 50% 100px;
 } .waveAnimation .waveTop {
     animation: move-wave 3s;
     -webkit-animation: move-wave 3s;
     -webkit-animation-delay: 1s;
     animation-delay: 1s;
 } .waveMiddle {
     background-size: 50% 120px;
 } .waveAnimation .waveMiddle {
     animation: move_wave 10s linear infinite;
 } .waveBottom {
     background-size: 50% 100px;
 } .waveAnimation .waveBottom {
     animation: move_wave 15s linear infinite;
 }
```