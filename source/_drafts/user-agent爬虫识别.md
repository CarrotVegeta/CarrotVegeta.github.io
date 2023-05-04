---
abbrlink: ''
categories:
- - http
date: '2023-05-04T22:43:31.172945+08:00'
tags:
- 爬虫
title: user-agent爬虫识别
updated: 2023-5-4T22:43:34.629+8:0
---
# user-agent爬虫识别

用户代理 （User Agent，简称 UA），是一个[特殊字符](https://baike.baidu.com/item/%E7%89%B9%E6%AE%8A%E5%AD%97%E7%AC%A6/112715?fromModule=lemma_inlink)串头，使得服务器能够识别客户使用的操作系统及版本、[CPU](https://baike.baidu.com/item/CPU/120556?fromModule=lemma_inlink) 类型、浏览器及版本、浏览器[渲染引擎](https://baike.baidu.com/item/%E6%B8%B2%E6%9F%93%E5%BC%95%E6%93%8E/10982158?fromModule=lemma_inlink)、浏览器语言、[浏览器插件](https://baike.baidu.com/item/%E6%B5%8F%E8%A7%88%E5%99%A8%E6%8F%92%E4%BB%B6/8330255?fromModule=lemma_inlink)等。

浏览器User-Agent通常由浏览器标识、渲染引擎标识、版本信息这三部分来构成。我们可以在这个位置来查看我们的User-Agent请求头值。

当我们需要做反爬虫时就可以拿这个字段来判断该请求是否是否来自真正的浏览器，当然这个是可以绕过的，很多爬虫程序会通过修改这个请求头来骗过我们的识别，所以我们需要在该情况下做一些加强的识别。

1、增加访问频率限制，当某个同样的User-Agent在单位时间内访问频率过高，加入黑名单，限制其访问.

2、
