---
layout: post
title:  "update web elements in chrome console with jquery"
date:   2017-11-10 20:34:24 +0800
categories: web jquery
tags: chrome jquery web
---
<h2>Table of contents</h2>
* TOC
{:toc}

[参考文档](https://developers.google.com/web/tools/chrome-devtools/console/command-line-reference)
## 使用场景
有时候需要临时找一个截图， 也不会PS， 下载全部静态页面到本地吧发现缺胳膊少腿， 各种js问题，css问题， 图片问题， 好在chrome里的开发者工具inspect很好用， 不过也有一个问题， 就是要一个一个的点击鼠标， 作为程序员是不能忍受的。 chrome里有console， 就是批量执行js脚本的好地方， 我们这里就用jQuery

## 验证chrome是否支持jQuery
```js
$ === jQuery
```

如果不支持可以人工输入下面代码：
```js
(function(){var jQueryVersion="1";var a=document.createElement("script");a.src="//ajax.googleapis.com/ajax/libs/jquery/"+jQueryVersion+"/jquery.js";a.type="text/javascript";document.getElementsByTagName("head")[0].appendChild(a);})()
```

>  我这边显示true， 所以没有测试上面的js function

## 在chrome中找到需要修改的元素， 点inspect, 复制Selector路径
![image](https://user-images.githubusercontent.com/150418/33536827-7941f48c-d8f2-11e7-8bfc-234cb3850e23.png)


## jQuery更新元素
```js
$("body > div.con > div.leftCon > div.item_con.pos_info > div.pos_base_statistics > span.pos_base_num.pos_base_update > span > strong")[0].innerHTML = "99"

```

