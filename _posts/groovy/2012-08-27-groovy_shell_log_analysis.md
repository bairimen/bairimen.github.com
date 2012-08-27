---
layout: post
title: "使用shell脚本结合groovy分析搜索日志获取热搜词"
description: ""
category: shell 
tags: shell groovy log search nginx
---

#有个需求，分析搜索日志，获取热搜词。

Nginx日志格式如下

`
8.8.8.8 [27/Aug/2012:00:21:17 +0800] "GET /search.do?callback=jsonp_callback&q=%E6%9C%89%E7%82%B9%E8%88%8D%E4%B8%8D%E5%BE%97&page=1&size=12 HTTP/1.1" 200 197 "Mozilla/5.0 (Linux; U; Android 4.0.3; zh-cn; ThL V12 Build/IML74K) AppleWebKit/534.30 (KHTML, like Gecko) Version/4.0 Mobile Safari/534.30"
`

其中*q=*后面部分为用户查询词。分两步走

##使用shell直接统计出topN查询词


<script src="https://gist.github.com/3488794.js?file=getHotSearch"></script>

words.sort 结果如下
`
$head words.sort
  38070 %E4%B8%AD%E5%9B%BD%E5%A5%BD%E5%A3%B0%E9%9F%B3
  37326 %E5%A6%82%E6%9E%9C%E6%B2%A1%E6%9C%89%E4%BD%A0
  33144 %E5%87%A4%E5%87%B0%E4%BC%A0%E5%A5%87
  33105 %E6%88%91%E7%9A%84%E6%AD%8C%E5%A3%B0%E9%87%8C
  32842 %E5%BC%A0%E5%AD%A6%E5%8F%8B
  32742 %E5%91%A8%E6%9D%B0%E4%BC%A6
  30867 %E9%99%88%E5%A5%95%E8%BF%85
  28990 %E5%88%98%E5%BE%B7%E5%8D%8E
  26416 %E6%9B%B2%E5%A9%89%E5%A9%B7
`



##调用groovy脚本入库，再次感慨下脚本的快捷

<script src="https://gist.github.com/3488814.js?file=save2db"></script>

OK.That's All.

