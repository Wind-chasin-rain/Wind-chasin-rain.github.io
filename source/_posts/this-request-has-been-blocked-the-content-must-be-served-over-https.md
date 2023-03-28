---
title: This request has been blocked； the content must be served over HTTPS.
date: 2022-06-08 13:31:54
cover: https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206081335064.png
tags: http
categories: 疑难杂症
description: This request has been blocked； the content must be served over HTTPS.
updated:
mathjax:
highlight_shrink:
---

## 错误信息

```css
This request has been blocked; the content must be served over HTTPS.
```

##   错误原因

1、Http与Https混合使用
        项目生产环境使用的是https协议，某页面嵌入第三方连接，通过ajax调用相关接口获取信息实现登陆第三方系统，第三方系统暂未升级使用https，故在页面点击链接时出现上述错误信息。

## 解决方法

1、将所有系统的协议都升级为https协议；

2、在引入第三方url的页面添加如下信息：

通过在网页 head 中添加标签

```css
<meta http-equiv="Content-Security-Policy" content="upgrade-insecure-requests">
```