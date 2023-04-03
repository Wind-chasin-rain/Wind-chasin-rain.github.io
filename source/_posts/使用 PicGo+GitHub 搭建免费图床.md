---
title: 使用 PicGo+GitHub 搭建免费图床
date: 2022-06-07 14:02:38
cover: https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206071404762.jpeg
tags: 
  - 图床
  - GitHub
categories: 技术分享
description: 使用 PicGo+GitHub 搭建免费图床
updated: 
mathjax:
highlight_shrink:
---

## 登录GitHub，创建一个新的仓库（public）；

![picgo01](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206071400466.png)

## 安装开源图床工具 [PicGo](https://molunerfinn.com/PicGo/)

打开官网，点击免费下载，在Assets下面选择合适的版本下载安装。
![picgo02](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206071401336.png)

## 配置PicGo

### Github 创建一个 token

在GitHub头像处打开 `Settings -> Developer settings -> Personal access tokens`，最后点击 `generate new token`；

### 填写及勾选相关信息

然后点击 `Genetate token` 即可

==勾选repo==

### token生成后自行保存

注意它只会显示一次

### 配置PicGo

依次打开 图床设置 -> Github 图床；
填写相关信息，最后点击 `确定`即可，要将其作为默认图床的话，点击设为默认图床；
<u>***分支名填master***</u>（若后续出现问题，可改成main试试）
![picgo03](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206071401531.png)

### 上传图片，通过上传区上传即可

Ctrl V 或者将图片拖拽都可以，也可以通过快捷键的方式上传（默认上传键为 `Ctrl + Shift + P`）

## ~~加速访问~~（貌似失效）

用 [jsDelivr](https://www.jsdelivr.com/) 进行免费加速，在我们 PicGo 图床配置中添加如下自定义域名即可；

------

​          https://cdn.jsdelivr.net/gh/用户名/仓库名@master

------



## 快捷键修改

推荐改成 `Ctrl + shift +c`。

![picgo快捷键](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206071402109.png)

