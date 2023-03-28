---
title: 使用Hexo+GitHub搭建个人博客
date: 2022-06-07 14:35:56
cover: https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206071409227.jpeg
tags: 
  - Hexo
  - GitHub
categories: 技术分享
description: 使用Hexo+Github搭建个人博客
updated:
mathjax:
highlight_shrink:
---

## 安装相关工具

### 安装Node.js

### 安装Git

打开cmd命令行(win+r 输入cmd回车)分别执行

```markdown
node -v
npm -v
git --version
```

如果都可以成功运行出现版本信息证明安装成功。

### 安装Hexo

必须按照步骤来，因为hexo需要使用node.js的npm命令。
在合适的地方新建一个文件夹，用来存放自己的博客文件，在目录下右键点击`Git Bash Here`，然后输入

```markdown
npm i hexo-cli -g
```

等待安装hexo完成后，输入

```markdown
hexo -v
```

检查是否安装成功。

### 在github上创建并设置远程库

### 初始化Hexo

```markdown
hexo init C:/hexo 
cd C:/hexo
npm install
```

依次输入上面三条语句

接着输入`hexo g`生成静态网页，然后输入`hexo s`打开本地服务器，然后浏览器打开http://localhost:4000/，效果如下：

[![X0L9pD.png](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206071437977.png)](https://imgtu.com/i/X0L9pD)

## 连接Github与本地

回到你的git bash中，

```
git config --global user.name "yourname"
git config --global user.email "youremail"
```

这里的yourname输入你的GitHub用户名，youremail输入你GitHub的邮箱。这样GitHub才能知道你是不是对应它的账户。

可以用以下两条，检查一下你有没有输对

```
git config user.name
git config user.email
```

然后创建SSH,一路回车

```
ssh-keygen -t rsa -C "youremail"
```

这个时候它会告诉你已经生成了.ssh的文件夹。在你的电脑中找到这个文件夹。

[![X0X8YV.png](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206071437978.png)](https://imgtu.com/i/X0X8YV)

ssh，简单来讲，就是一个秘钥，其中，id_rsa是你这台电脑的私人秘钥，不能给别人看的，id_rsa.pub是公共秘钥，可以随便给别人看。把这个公钥放在GitHub上，这样当你链接GitHub自己的账户时，它就会根据公钥匹配你的私钥，当能够相互匹配时，才能够顺利的通过git上传你的文件到GitHub上。

而后在GitHub的setting中，找到SSH keys的设置选项，点击New SSH key
把你的id_rsa.pub里面的信息复制进去。

[![X0jQje.png](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206071437979.png)](https://imgtu.com/i/X0jQje)

在gitbash中，查看是否成功

```
ssh -T git@github.com
```



## 将hexo部署到GitHub

这一步，我们就可以将hexo和GitHub关联起来，也就是将hexo生成的文章部署到GitHub上，打开站点配置文件 _config.yml，翻到最后，修改为
YourgithubName就是你的GitHub账户

```
deploy:
  type: git
  repo: https://github.com/YourgithubName/YourgithubName.github.io.git
  branch: master
```

这个时候需要先安装deploy-git ，也就是部署的命令,这样你才能用命令部署到GitHub。

```
npm install hexo-deployer-git --save
```

然后

```
hexo clean
hexo generate
hexo deploy
```

其中 hexo clean清除了你之前生成的东西，也可以不加。
hexo generate 顾名思义，生成静态文章，可以用 hexo g缩写
hexo deploy 部署文章，可以用hexo d缩写

注意deploy时可能要你输入username和password。

得到下图就说明部署成功了，过一会儿就可以在http://yourname.github.io 这个网站看到你的博客了！！

[![X0v12T.png](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206071437980.png)](https://imgtu.com/i/X0v12T)



## 设置个人域名

现在你的个人网站的地址是 yourname.github.io，如果觉得这个网址逼格不太够，这就需要你设置个人域名了。但是需要花钱。

你需要先去进行实名认证,然后在域名控制台中，看到你购买的域名。

点解析进去，添加解析。

其中，192.30.252.153 和 192.30.252.154 是GitHub的服务器地址。
解析线路选择默认。

![image-20220607114636904](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206071146026.png)

登录GitHub，进入之前创建的仓库，点击settings，设置Custom domain，输入你的域名

![image-20220607114800278](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206071148365.png)

然后在你的博客文件source中创建一个名为CNAME文件，不要后缀。写上你的域名。

![image-20220607114907178](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206071149229.png)

最后，在gitbash中，输入

```
hexo clean
hexo g
hexo d
```


过不了多久，再打开你的浏览器，输入你自己的域名，就可以看到搭建的网站啦！

接下来你就可以正式开始写文章了。

```
hexo new newpapername
```

然后在source/_post中打开markdown文件，就可以开始编辑了。当你写完的时候，再

```
hexo clean
hexo g
hexo d
```



就可以看到更新了。