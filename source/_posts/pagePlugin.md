---
title: Jquery分页插件
cover: http://sora3.coding.me/imgs/preview/preview1.jpg
---
## html结构
``` bash
<ul class="pagination" id="page">
</ul>
<div class="pageJump">
    <span>跳转到</span>
    <input type="text" />
    <span>页</span>
    <button type="button" class="button">确定</button>
</div>
```
## css样式
```bash
ul {
    list-style: none;
    padding: 0;
    margin: 0;
}
.pagination{
	font-size: 14px;
	display:inline-block;
	padding-left:0;
	margin: 36px 10px 20px 30%;
	border-radius:4px
}
.pagination>li{display:inline}  
.pagination>li>a,.pagination>li>span{
	position:relative;
	float:left;
	padding:6px 12px;
	margin-left:-1px;
	line-height:1.42857143;
	color:#ff7a45;
	text-decoration:none;
	background-color:#fff;
	border:1px solid #ddd;
	cursor: pointer;
	-webkit-touch-callout: none;
	-webkit-user-select: none;
	-khtml-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;
}
.pagination>li:first-child>a,.pagination>li:first-child>span{
	margin-left:0;
	border-top-left-radius:4px;
	border-bottom-left-radius:4px;
}  
.pagination>li:last-child>a,.pagination>li:last-child>span{
	border-top-right-radius:4px;
	border-bottom-right-radius:4px;
}  
.pagination>li>a:focus,.pagination>li>a:hover,.pagination>li>span:focus,.pagination>li>span:hover{
	z-index:2;
	color:#23527c;
	background-color:#eee;
	border-color:#ddd;
}  
.pagination>.active>a,.pagination>.active>a:focus,.pagination>.active>a:hover,.pagination>.active>span,.pagination>.active>span:focus,.pagination>.active>span:hover{
	z-index:3;color:#fff;
	cursor:default;
	background-color:#ff7a45;
	border-color:#ff7a45;
}  
.pagination>.disabled>a,.pagination>.disabled>a:focus,.pagination>.disabled>a:hover,.pagination>.disabled>span,.pagination>.disabled>span:focus,.pagination>.disabled>span:hover{
	color:#777;
	cursor:not-allowed;
	background-color:#fff;
	border-color:#ddd;
}  
.pagination-lg>li>a,.pagination-lg>li>span{
	padding:10px 16px;
	font-size:18px;
	line-height:1.3333333;
}  
.pagination-lg>li:first-child>a,.pagination-lg>li:first-child>span{
	border-top-left-radius:6px;
	border-bottom-left-radius:6px;
}  
.pagination-lg>li:last-child>a,.pagination-lg>li:last-child>span{
	border-top-right-radius:6px;
	border-bottom-right-radius:6px;
}  
.pagination-sm>li>a,.pagination-sm>li>span{
	padding:5px 10px;
	font-size:12px;
	line-height:1.5;
}  
.pagination-sm>li:first-child>a,.pagination-sm>li:first-child>span{
	border-top-left-radius:3px;
	border-bottom-left-radius:3px;
}  
.pagination-sm>li:last-child>a,.pagination-sm>li:last-child>span{
	border-top-right-radius:3px;
	border-bottom-right-radius:3px;
}  
.pager{
	padding-left:0;
	margin:20px 0;
	text-align:center;
	list-style:none;
}  
.pager li{
	display:inline;
}  
.pager li>a,.pager li>span{
	display:inline-block;
	padding:5px 14px;
	background-color:#fff;
	border:1px solid #ddd;
	border-radius:15px;
}  
.pager li>a:focus,.pager li>a:hover{
	text-decoration:none;
	background-color:#eee;
}  
.pager .next>a,.pager .next>span{
	float:right;
}  
.pager .previous>a,.pager .previous>span{
	float:left;
}  
.pager .disabled>a,.pager .disabled>a:focus,.pager .disabled>a:hover,.pager .disabled>span{
	color:#777;
	cursor:not-allowed;
	background-color:#fff;
}
.pageJump {
	display: inline-block;
	padding-left: 0;
	margin: 22px 10px;
	border-radius: 4px;
	vertical-align: top;
	font-size: 14px;
}
.pageJump .button,.pageJump input{
	font-size: 16px;
	padding: 2px 4px;
	margin-left: -1px;
	line-height: 1.42857143;
	color: #ff7a45;
	text-decoration: none;
	background-color: #fff;
	border: 1px solid #ddd;
}
.pageJump input{
	width:50px;
}
.pageJump-lg .button,.pageJump-lg input{
	padding:10px 16px;
	font-size:18px;
	line-height:1.3333333;
}
.pageJump-sm .button,.pageJump-sm input{
	padding:5px 10px;
	font-size:12px;
	line-height:1.5;
}
```
## js
```bash
/**
 *  分页
 * @param opt
 * @constructor
 */
function Page(opt){
var set = $.extend({num:null,startnum:1,elem:null,callback:null},opt||{});
if(set.startnum>set.num||set.startnum<1){
set.startnum = 1;
}
var n = 0,htm = '';
var clickpages = {
	elem:set.elem,
	num:set.num,
	callback:set.callback,
	init:function(){
		this.elem.next('div.pageJump').children('.button').unbind('click')
		this.JumpPages();
		this.elem.children('li').click(function () {
			var txt = $(this).children('a').text();
			var page = '', ele = null;
			var page1 = parseInt(clickpages.elem.children('li.active').attr('page'));
	if (isNaN(parseInt(txt))) {
		switch (txt) {
			case '下一页':
		if (page1 == clickpages.num) {
			return;
		}
		if (page1 >= (clickpages.num - 2) || clickpages.num <= 6 || page1 < 3) {
			ele = clickpages.elem.children('li.active').next();
		} else {
			clickpages.newPages('next', page1 + 1);
			ele = clickpages.elem.children('li.active');
		}
		break;
	case '上一页':
	if (page1 == '1') {
		return;
}
		if (page1 >= (clickpages.num - 1) || page1 <= 3 || clickpages.num <= 6) {
			ele = clickpages.elem.children('li.active').prev();
		} else {
			clickpages.newPages('prev', page1 - 1);
			ele = clickpages.elem.children('li.active');
		}
		break;
	case '«':
		if (page1 == '1') {
			return;
		}
		if (clickpages.num > 6) {
			clickpages.newPages('«', 1);
		}
		ele = clickpages.elem.children('li[page=1]');
		break;
	case '»':
		if (page1 == clickpages.num) {
			return;
		}
	if (clickpages.num > 6) {
		clickpages.newPages('»', clickpages.num);
	}
	ele = clickpages.elem.children('li[page=' + clickpages.num + ']');
	break;
case '...':
	return;
	}
} else {
	if ((parseInt(txt) >= (clickpages.num - 3) || parseInt(txt) <= 3) && clickpages.num > 6) {
		clickpages.newPages('jump', parseInt(txt));
	}
	ele = $(this);
}
page = clickpages.actPages(ele);
if (page != '' && page != page1) {
	if (clickpages.callback){
		clickpages.callback(parseInt(page));
	}
}
	});
},
//active
actPages:function (ele) {
	ele.addClass('active').siblings().removeClass('active');
	return clickpages.elem.children('li.active').text();
},
JumpPages:function () {
	this.elem.next('div.pageJump').children('.button').click(function(){
		var i = parseInt($(this).siblings('input').val());
		if(isNaN(i)||(i<=0)||i>clickpages.num){
			return;
		}else if(clickpages.num>6){
			clickpages.newPages('jump',i);
		}else{
			var ele = clickpages.elem.children('li[page='+i+']');
			clickpages.actPages(ele);
			if (clickpages.callback){
				clickpages.callback(i);
			}
			return;
		}
		
		if (clickpages.callback){
			clickpages.callback(i);
		}
	})
},
//newpages
newPages:function (type, i) {
	var html = "",htmlLeft="",htmlRight="",htmlC="";
	var HL = '<li><a>...</a></li>';
	html = '<li><a  aria-label="Previous">&laquo;</a></li>\
		<li><a>上一页</a></li>'
	for (var n = 0;n<3;n++){
		htmlC += '<li '+((n-1)==0?'class="active"':'')+' page="'+(i+n-1)+'"><a>'+(i+n-1)+'</a></li>';
		htmlLeft += '<li '+((n+2)==i?'class="active"':'')+' page="'+(n+2)+'"><a>'+(n+2)+'</a></li>';
		htmlRight += '<li '+((set.num+n-3)==i?'class="active"':'')+' page="'+(set.num+n-3)+'"><a>'+(set.num+n-3)+'</a></li>';
	}
	
witch (type) {
case "next":
	if(i<=4){
		html+='<li page="1"><a>1</a></li>'+htmlLeft+HL+'<li page="'+set.num+'"><a>'+set.num+'</a></li>';
	}else if(i>=(set.num-3)){
		html+='<li page="1"><a>1</a></li>'+HL+htmlRight+'<li page="'+set.num+'"><a>'+set.num+'</a></li>';
	}else{
		html += '<li page="1"><a>1</a></li>'+HL+htmlC+HL+'<li page="'+set.num+'"><a>'+set.num+'</a></li>';
	}
	break;
case "prev":
	if(i<=4){
		html+='<li page="1"><a>1</a></li>'+htmlLeft+HL+'<li page="'+set.num+'"><a>'+set.num+'</a></li>';
	}else if(i>=(set.num-3)){
		html+='<li page="1"><a>1</a></li>'+HL+htmlRight+'<li page="'+set.num+'"><a>'+set.num+'</a></li>';
	}else{
		html += '<li page="1"><a>1</a></li>'+HL+htmlC+HL+'<li page="'+set.num+'"><a>'+set.num+'</a></li>';
	}
	break;
case "«" :
	html+='<li class="active" page="1"><a>1</a></li>'+htmlLeft+HL+'<li page="'+set.num+'"><a>'+set.num+'</a></li>';
	break;
case "»" :
	html+='<li page="1"><a>1</a></li>'+HL+htmlRight+'<li class="active" page="'+set.num+'"><a>'+set.num+'</a></li>';
	break;
case "jump" :
	if(i<=4){
		if(i==1){
			html+='<li class="active" page="1"><a>1</a></li>'+htmlLeft+HL+'<li page="'+set.num+'"><a>'+set.num+'</a></li>';
		}else{
			html+='<li page="1"><a>1</a></li>'+htmlLeft+HL+'<li page="'+set.num+'"><a>'+set.num+'</a></li>';
		}
	}else if((i>=set.num-3)&&(set.num>=7)){
		if(i==set.num){
			html+='<li page="1"><a>1</a></li>'+HL+htmlRight+'<li class="active" page="'+set.num+'"><a>'+set.num+'</a></li>';
		}else{
			html+='<li page="1"><a>1</a></li>'+HL+htmlRight+'<li page="'+set.num+'"><a>'+set.num+'</a></li>';
		}
	}else{
		html += '<li page="1"><a>1</a></li>'+HL+htmlC+HL+'<li page="'+set.num+'"><a>'+set.num+'</a></li>';
	}
		}
		html += '<li><a>下一页</a></li>\
			<li><a  aria-label="Next">&raquo;</a></li>';
		if (this.num > 5 || this.num < 3) {
			set.elem.html(html);
			clickpages.init({num:set.num,elem:set.elem,callback:set.callback});
		}
	}
}
if(set.num<=1){
	$(".pagination").html('');
	return;
}else if(parseInt(set.num)<=6){
	n = parseInt(set.num);
	var html='<li><a  aria-label="Previous">&laquo;</a></li>\
			<li><a>上一页</a></li>';
	for(var i=1;i<=n;i++){
		if(i==set.startnum){
			html+='<li class="active" page="'+i+'"><a>'+i+'</a></li>';
		}else{
			html+='<li page="'+i+'"><a>'+i+'</a></li>';
		}
	}
	html +='<li><a>下一页</a></li>\
			<li><a  aria-label="Next">&raquo;</a></li>';
	set.elem.html(html);
	clickpages.init();
}else{
	clickpages.newPages("jump",set.startnum)
}


```

## 使用方法
```bash
 Page({
     num: pages, //页码数
     startnum: page, //指定页码
     elem: $('#page'), //指定的元素
     callback: function (num) { //回调函数
        num当前数量
     }
  });
```