---
title: 用Docker部署GeminiPro Chat项目
date: 2023-12-29 17:07:55
cover: https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202312291714519.png
tags:
  - Docker
updated:
categories: 技术分享
description: 用Docker和谷歌Gemini-API私有化部署GeminiPro Chat项目
mathjax:
toc: true
toc_number: false
---



# Demo

https://gg.windcrain.top/

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202312291707986.png)

# 准备条件

1）一台服务器

2）此项目的github
https://github.com/babaohuang/GeminiProChat

3）一个谷歌账号
用来获取免费的API
https://makersuite.google.com/app/apikey

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202312291555312.png)



# 一、Docker环境部署

**docker安装脚本**

```bash
bash <(curl -sSL https://cdn.jsdelivr.net/gh/SuperManito/LinuxMirrors@main/DockerInstallation.sh)
```

**docker-compose安装脚本**

```
curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && chmod +x /usr/local/bin/docker-compose
```

</br>

# 二、创建docker-compose.yml文件

```bash
sudo -i #切换到root用户

mkdir -p /root/data/docker_data/GeminiProChat #创建一个目录

cd /root/data/docker_data/GeminiProChat

vim docker-compose.yml  

#英文输入法下，按 i进入编辑
#esc退出编辑
# :wq保存退出
# :q退出  :q!强制退出
```

```yaml
version: '3.3'  # 这是一个Docker Compose文件的版本声明，它表明该文件符合Docker Compose文件格式版本3.3
services:
    ywsj_gemini:   #服务名，可以自定义
        container_name: ywsj_gemini    #容器名，可以自定义
        ports:
            - '3000:3000'   # 冒号:左边的3000可以改成任意vps上未使用过的端口记得打开防火墙,冒号右边是本docker镜像里的端口
        environment:
            - PUID=0    # 用户ID,在终端输入id可以查看当前用户的id
            - PGID=0    # 组ID同上
            - TZ=Asia/Shanghai  #时区，可以自定义
            - GEMINI_API_KEY=AIlls3SyAZAoLx6TO2W40ojhWUJf_uqEKwGp5NqFU   #这里填自己准备条件上获取到的谷歌AI的API
        restart: always    #开启自启动其他选项看以下备注
        image: babaohuang/geminiprochat:latest    #镜像名一般都是使用的哪个镜像就写哪个镜像。

```

</br>

# 三、执行容器运行命令

```bash
docker-compose up -d #运行容器

docker-compose ps  #查看是否开启成功

```

</br>

#  四、打开web页面使用

```bash
http://ip:8181   #打开自己VPS的端口加ip进入web页面

#可以绑定域名来访问
```

</br>

# 五、更新网站

```bash
cd /root/data/docker_data/GeminiProChat   #进入项目目录

docker-compose down #停止容器

docker-compose pull #拉取最新镜像

docker-compose up -d #启动新容器
```