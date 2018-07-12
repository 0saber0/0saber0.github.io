---
layout: post
title: JavaScript-JS中的Boolean
date: 2018-06-29 14:34:24
categories: JavaScript
tags: JavaScript boolean
author: 密拉诺
---

* content
{:toc}

## JS中的Boolean

```javascript
	var boolean1 = true;
	var boolean2 = false;

	console.log(typeof boolean1); //boolean
	console.log(typeof boolean2); //boolean
```

　　虽然Boolean类型的字面值只有两个，但ECMAScript中所有类型的值都有与这两个Boolean值等价的值：

　　- 任何非零数值都是true，包括正负无穷大，只有0和NaN是false
　　- 任何非空字符串都是true，只有空字符串是false
　　- 任何对象都是true，只有null和undefined是false

```javascript
	var boolean1 = Boolean(0);
	console.log(boolean1); //false

	var boolean2 = Boolean(1);
	console.log(boolean2); //true

	var boolean3 = Boolean(-1);
	console.log(boolean3); //true

	var boolean4 = Boolean("hello");
	console.log(boolean4); //true

	var boolean5 = Boolean("");
	console.log(boolean5); //false

	var boolean6 = Boolean(undefined);
	console.log(boolean6); //false

	var boolean7 = Boolean(null);
	console.log(boolean7); //false
```

利用上面所得到的结论，我们在JS中判断数据是否为空时就会简化,下面是一个简单的演示：

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>用户登录</h1>
	<form action="/login" method="post">
		账号：<input type="text" id="username" name="username" value="" />
		<br>
		密码：<input type="password" id="password" name="password" value="" />
		<br>
		<input type="button" value="登录" onclick="checklogin()"/>
	</form>

	<script type="text/javascript">
		function checklogin(){
			var username = document.getElementById("username").value;
			var password = document.getElementById("password").value;
			//判断账号是否为空
			if(username == ""){
				alert("账号不能为空");
				return;
			}
			//判断密码是否为空
			if(password == ""){
				alert("请输入密码");
				return;
			}
			//提交表单
			document.getElementsByTagName("form")[0].submit();
		}
	</script>
</body>
</html>
```

上面的JS中的判断可以直接简化：

```javascript
			//判断账号是否为空
			if(!username){
				alert("账号不能为空");
				return;
			}
			//判断密码是否为空
			if(!password){
				alert("请输入密码");
				return;
			}
```
这样的效果和上面一样，但看起来要相对简单一些。