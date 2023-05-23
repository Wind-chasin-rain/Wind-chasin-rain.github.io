---
title: Git快速入门
date:  2023-05-23 15:05:08
cover: https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202305231507952.jpg
tags: git
updated:
categories: 学习笔记
description: 简单的Git入门指南
mathjax:
toc: false
toc_number: false
---

Git是一种分布式版本控制系统，用于跟踪代码的变化和版本历史并支持多人协作开发。它已成为许多软件开发团队的首选工具，并被广泛使用于各种类型的项目中。

------

# 版本控制

版本控制又称为版本管理，是一种记录文件内容变化，以及对这些变化进行跟踪和控制的技术。它主要应用于软件开发、文档编写等领域，旨在协助多人协作开发或编辑同一项目时有效地管理文件的变化历史和版本。

版本控制系统通常可以帮助团队记录文件的每一个版本，即可跟踪整个项目的历史，并允许开发人员回滚到之前的版本，查看某个特定版本的变化，比较不同版本之间的差异等。同时，版本控制系统也能够处理文件的冲突，避免不同开发人员之间出现代码混乱、重复或矛盾的情况。

<br>

# 版本控制软件

版本控制软件的基础**功能**包括：

1. 提供源代码管理：版本控制软件能够管理和跟踪文件的历史版本，包括源代码、二进制文件、文本文件等。
2. 支持多人协作开发：版本控制软件允许多个开发人员在同一代码库或文件上进行协作开发。它可以帮助团队协调不同人员之间的工作，标记代码更改，并合并代码。
3. 版本历史记录：版本控制软件可以跟踪每个文件在整个开发过程中的变化历史，记录了每个版本或提交的详细信息，例如时间、作者、注释等。
4. 分支和合并：版本控制软件支持分支和合并，使开发人员可以独立地开发新功能或修复错误，而不干扰其他开发人员的工作。它还可以将不同的代码分支合并到一个通用的代码库中。
5. 冲突解决：版本控制软件可以标记由于同时修改同一个文件而产生的冲突，并提供工具来解决这些冲突。
6. 标签和版本号：版本控制软件允许开发人员对重要版本进行标记和归档，以便于查找和恢复早期版本。它还可以为每个提交分配唯一的版本号，以便于跟踪和管理。

版本控制软件可以分为两种不同的**类型**：集中式和分布式。

1. 集中式版本控制软件（Centralized Version Control System, CVCS）：集中式版本控制系统将所有文件存储在中央仓库中，并且开发人员需要从该中央仓库中获得最新版本，然后开始工作。开发人员进行更改后，必须将其提交回中央仓库。CVCS通常具有较好的稳定性和可靠性，但是当多个开发人员同时访问同一个文件时，可能会发生冲突，需要手动解决。
2. 分布式版本控制软件（Distributed Version Control System, DVCS）：分布式版本控制系统允许每个开发人员都拥有完整的代码库副本，并可以在本地对代码进行更改，而不必连接到中央服务器。开发人员可以方便地保存本地历史记录并跟踪文件变化。当需要与其他开发人员共享代码时，可以通过合并来处理更改冲突。DVCS通常具有更高的灵活性和可伸缩性，但也需要更多的硬盘空间和计算能力。

常见的集中式版本控制软件包括Subversion (SVN)、Perforce、ClearCase等；而Git、Mercurial等则是分布式版本控制软件的代表。在选择使用哪种版本控制软件时，需要考虑团队规模、开发流程、安全性、可用性等方面的需求和限制。

<br>

# 安装

Git下载：https://git-scm.com/downloads 

GitHub Desktop下载：https://desktop.github.com/

IDEA集成Github：`VCS`  ~>   `Share Project on Github`

<br>

# 命令

以下是一些Git常用指令的代码：

- 初始化Git仓库

```
git init
```

- 克隆一个远程Git仓库

```
git clone [url]
```

- 添加文件到Git仓库

```
git add [file]
```

- 提交更改到Git仓库

```
git commit -m [message]
```

- 查看当前Git仓库状态

```
git status
```

- 查看Git仓库中文件变化

```
git diff [file]
```

- 查看Git提交历史记录

```
git log
```

- 恢复Git仓库到某个历史版本

```
git restore [commit]
```

- 创建新的Git分支

```
git branch [branch-name]
```

- 切换到指定Git分支

```
git checkout [branch-name]
```

- 创建并切换到指定Git分支

```
git checkout -b [branch-name]
```

- 合并指定Git分支到当前分支

```
git merge [branch-name]
```

- 删除Git分支

```
git branch -d [branch-name]
```

+ 从本地Git仓库推送更改到远程分支

```
git push [remote] [branch]
```

- 从远程Git仓库拉取更新到本地

```
git pull [remote] [branch]
```

- 设置Git全局配置信息

```
git config --global user.name "[name]"
git config --global user.email "[email]"
```

- 将本地仓库与远程仓库关联

```
git remote add origin [远程仓库URL]
```

这些Git指令只是Git命令集的一部分，涵盖了Git基本操作的核心指令。在实际使用中，还需要不断学习和掌握更多其他Git指令，以更好地管理和协作开发代码。