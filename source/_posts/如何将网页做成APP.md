---
title: 如何将网页做成APP
date:  2023-04-03 10:29:08
cover: https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202304031034466.png
tags: 
 - APP
 - 网站
updated:
categories: 技术分享
description: 本文将介绍如何通过iAPP将你的网站制作成APP
mathjax:
toc: true
toc_number: false
---

如果我们有一个网页需要经常访问，以打开浏览器输入网址或者点击书签的方式就让人感到麻烦。本文将介绍如何通过iAPP将你的网站制作成APP，只要点击就可以直接访问。

------

# iAPP

iAPP是一款在手机上制作APP的应用，可以在应用商店搜索到。



# 教程

1.打开iAPP，在左上角点击新建项目，输入标题，语言选择裕V3，创建应用

<img src="https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202304031035732.png" alt="20230403100208" style="zoom:25%;" />



2.点击**可视编程设计**我们就可以进入项目设计页面了



3.新项目建立后系统会自动生成一个“hello world”页面，我们可以直接在页面中删掉这个页面

<img src="https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202304031035733.png" alt="20230403100005" style="zoom:25%;" />



4.新页面，我们要添加一个浏览器控件，在右侧控件点开，找到**浏览器**控件进行添加

<img src="https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202304031035734.png" alt="20230403100657" style="zoom:25%;" />



5.在控件属性页面将控件高和宽都设置为**-1**（-1的意思为最大）



6.点击界面事件，添加一个**载入事件**



我们需要在载入事件面板中填写代码

```php
s a="https://www.doubt-fact.top"

//s申明一个变量,设置变量a为网址

us(1,"url",a)

//1为控件的代号
```



补充一点，如果需要每次访问都清理缓存的话可以添加这句话

```
hs("del cookie")
```



这样，我们就创建好了，测试一下，是不是可以访问了呢？

<img src="https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202304031035735.png" alt="20230403101701" style="zoom:25%;" />



7.在界面事件中添加**按键按下事件**



输入以下代码

```php
f(st_kC==4)

//按下返回键

{

  ug(1,"cangoback",c)

  //判断是否存在可以后退的网页

 f(c==true)

  {

  //如果存在

   us(1,"gobackorforward",-1)

  //网页后退一步（这里整数为前进,负数为后退）

  }

  else

  //如果不存在

  {

   ends()

  //退回桌面

  }

}
```



保存退出设计

<img src="https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202304031035736.png" alt="20230403102113" style="zoom:25%;" />



8.点击**权限配置管理**，将访问网络的权限打开，**打包测试**

<img src="https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202304031035737.png" alt="20230403102525" style="zoom:25%;" />



安装APP即可使用

<img src="https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202304031035738.png" alt="16" style="zoom:25%;" />

