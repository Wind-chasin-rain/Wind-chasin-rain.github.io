---
title: OpenSSL SSL_read：Connection was reset, errno 10054
date: 2023-03-23 19:57:55
cover: https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202303232253670.png
tags: git
updated:
categories: 疑难杂症
---

**<big>❌  git 报错信息 ：</big>**

```
OpenSSL SSL_read: Connection was reset, errno 10054 ...
```

**<big>❗   异常信息</big>**

`Git Bash` 中，`push` 或者`hexo deploy`时，出现错误

```c
git push

hexo d
```

**<big>⭕  解决方案</big>**

1.邮箱问题

查看用户名，邮箱

```c
git config user.name
git config user.email
```

修改，用户名，邮箱

```c
git config --global user.name "xxx"
git config --global user.email "xxx"
```

移除仓库，重新添加

```c
git remote rm origin
git remote add origin https://github.com/XXX
```

2.解除SSL认证

在 Git Bash 中输入以下命令：

```c
git config --global http.sslVerify "false"
```

3.更新 DNS 缓存

cmd 窗口输入

```c
ipconfig /flushdns
```

4.文件过大，超过上限

修改为 500MB，在 Git Bash 中输入以下命令：

```C
git config http.postBuffer 5242880003
```

