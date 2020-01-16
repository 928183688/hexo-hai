---
title: 常用的正则表达式
cover: https://upload.dianshangdanao.com/202001101224105672.jpg
---

## 上代码
```js
const valid = {
	//验证手机号是否正确
	//parameter 1  需要验证的手机号
	'verifyPhone': function (param) {
		if (typeof param == 'string') {
			if (!$.trim(param)) return false;
			var phone = param;
			var Reg = /^[1][3,4,5,,6,7,8,9][0-9]{9}$/;
			return Reg.test(phone) ? true : false;
		} else {
			throw Error('传惨类型错误');
		}
	},
	//密码验证
	'verifyPassword': function (param) {
		if (typeof param == 'string') {
			if (!$.trim(param)) return false;
			var password = param;
			var Reg = /^[a-zA-Z]\w{5,17}$/;
			return Reg.test(password) ? true : false;
		} else {
			throw Error('传惨类型错误');
		}
	},
	//验证邮箱是否合法
	//parameter 1  需要验证的邮箱
	'verifyEmail': function (param) {
		if (typeof param == 'string') {
			if (!$.trim(param)) return false;
			var email = param;
			var Reg = /^([a-zA-Z0-9]+[_|\_|\.]?)*[a-zA-Z0-9]+@([a-zA-Z0-9]+[_|\_|\.]?)*[a-zA-Z0-9]+\.[a-zA-Z]{2,3}$/;
			return Reg.test(email) ? true : false;
		} else {
			throw Error('传惨类型错误');
		}
	},
	//判断是否为空   字符串或者对象都可以
	'isEmpty': function (param) {
		if (typeof param == 'string') param = $.trim(param);
		if (param == '' || typeof param == 'undefined' || param == null || param == undefined) {
			return true;
		} else {
			return false;
		}
	},
	//不能中文
	'isCheinese': function (param) {
		if (typeof param == 'string') {
			if (!$.trim(param)) return false;
			var password = param;
			var Reg = /.*[\u4e00-\u9fa5]+.*$/;
			return Reg.test(password) ? true : false;
		} else {
			throw Error('传惨类型错误');
		}
	},
	//起码8位以上
	'isNumber': function (param) {
		if (typeof param == 'string') {
			if (!$.trim(param)) return false;
			var password = param;
			var Reg = /^\d{8,}$/
			return Reg.test(password) ? true : false;
		} else {
			throw Error('传惨类型错误');
		}
	},
	//只能数字
	'mustNumber': function (param) {
		if (typeof param == 'string') {
			if (!$.trim(param)) return false;
			var password = param;
			var Reg = /^[0-9]*$/
			return Reg.test(password) ? true : false;
		} else {
			throw Error('传惨类型错误');
		}
	},
	//匹配特殊符号
	'special': function (param) {
		if (typeof param == 'string') {
			if (!$.trim(param)) return false;
			var Reg = /['&']/
			return Reg.test(param) ? true : false;
		} else {
			throw Error('传惨类型错误');
		}
	},
	//保留前三位 后面得*******
	'star': function (param) {
		if (typeof param == 'string') {
			var Reg = /^(\d{3})\d+(\d{4})$/
			// var Reg = /^(\d{4})\d+(\d{4})$/
			return param = param.replace(Reg, "$1********")
			// return param = param.replace(Reg, "$1****$2")
		} else {
			throw Error('传惨类型错误');
		}
	},
	// 校验15位的身份证号码
    'check15IdCardNo': function (idCardNo) {
	// 15位身份证号码的基本校验
	var check = /^[1-9]\d{7}((0[1-9])|(1[0-2]))((0[1-9])|([1-2][0-9])|(3[0-1]))\d{3}$/
		.test(idCardNo);
	if (!check)
		return false;
	// 校验地址码
	var addressCode = idCardNo.substring(0, 6);
	check = idCardNoUtil.checkAddressCode(addressCode);
	if (!check)
		return false;
	var birDayCode = '19' + idCardNo.substring(6, 12);
	// 校验日期码
	return idCardNoUtil.checkBirthDayCode(birDayCode);
    },
// 校验18位的身份证号码
    'check18IdCardNo': function (idCardNo) {
	// 18位身份证号码的基本格式校验
	var check = /^[1-9]\d{5}[1-9]\d{3}((0[1-9])|(1[0-2]))((0[1-9])|([1-2][0-9])|(3[0-1]))\d{3}(\d|x|X)$/
		.test(idCardNo);
	if (!check)
		return false;
	// 校验地址码
	var addressCode = idCardNo.substring(0, 6);
	check = idCardNoUtil.checkAddressCode(addressCode);
	if (!check)
		return false;
	// 校验日期码
	var birDayCode = idCardNo.substring(6, 14);
	check = idCardNoUtil.checkBirthDayCode(birDayCode);
	if (!check)
		return false;
	// 验证校检码
	return idCardNoUtil.checkParityBit(idCardNo);
	}
}
```