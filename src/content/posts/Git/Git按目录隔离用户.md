---
title: Git includeIf按目录隔离用户
published: 2026-04-20
image: api
category: 教程
tags: [Git, 配置]
---

# Git includeIf按目录隔离用户

## Git配置文件

> .gitconfig 配置文件路径 **C:\Users\\${username}**

全局配置文件内容

```text
[user]
	email = email@email.com
	name = default user
[credential "https://gitee.com"]
	provider = generic
[credential "http://192.168.8.248:6080"]
	provider = generic
[credential "http://111.22.163.233:36080"]
	provider = generic
	
# "F:/work/"路径下使用对应配置文件
# 需要使用"/" 不能使用"\"
[includeIf "gitdir/i:F:/work/"]
    path = .gitconfig-work
```

新建文件`.gitconfig-work`

```text
[user]
	email = work@email.com
	name = work user
```

**注：直接去目录下使用`git config --list`无法查看变化，需要对应目录下`commit`后才能查看到用户改变。`commit`会使用`includeIf`引入的配置，也可以`git log`查看。**

