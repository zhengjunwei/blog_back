---
title: 数组为什么不能用prototype判断
layout: page
category: sdk
date: 2014-06-06
---

```
	var iframe = document.createElement('iframe');
	document.body.appendChild(iframe);
	xArray = window.frames[window.frames.length - 1].Array;
	var arr = new xArray();
	console.log(arr instanceof Array);
```