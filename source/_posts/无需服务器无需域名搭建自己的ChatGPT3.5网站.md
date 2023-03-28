---
title: 无需服务器无需域名搭建自己的ChatGPT3.5网站
date: 
cover: https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202303241105919.png
tags: 
 - 网站
 - chatGPT
updated:
categories: 技术分享
mathjax:
toc: true
toc_number: false
---

# 准备条件

1. ChatGPT的API-KEY
   获取地址：
   https://platform.openai.com/account/api-keys

2. 注册好的github账号
   注册地址：
   https://github.com/

3. 注册好的vercel账号
   用github登录即可，需要手机号验证
   https://vercel.com/login

4. 本开源项目作者的GitHub
   https://github.com/ddiu8081/chatgpt-demo

# 搭建开始

## 1)打开这个项目

https://github.com/nezha001/chatgpt-ywsj
登录好自己的GitHub账号

## 2)点击项目左下侧的Deploy进入Vercel页面

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202303241152265.png)

## 3)然后用github登录成功(需要验证手机号)

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202303241154700.png)

## 4)选择github

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202303241153222.png)

然后自定义一个自己的名称-点击创建



## 5)填入自己的chatgpt的api-key，部署即可

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202303241200045.png)

## 6）大约等待1分钟即可成功

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202303241203899.png)

## 7) 点击Continue To Dashboard进入管理页面

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202303241208597.png)

点击`Visit`即可进入vercel官方分配的网址

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202303241211855.png)

# 绑定自定义域名（可选）

选中自己的项目-打开下图-View Domains

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202303242111478.png)

将自己要绑定的域名填入以下的位置并ADD增加

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202303242112332.png)

此时他会要求我们做如下CNAME

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202303242121663.png)

那么我们将自己的域名CNAME到vercel给你的域名即可

打开Cloudflare做如下配置

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202303242120271.png)

添加一条点下保存，再添加下一条

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202303242123056.png)

稍微等待一会，刷新，直到没有错误提示即可

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202303242124806.png)

此时(有可能还得等待一会5分钟)就可以用自己的域名来访问了



Demo:

https://www.mzlfreegpt.eu.org/

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202303242128213.png)
