---
title: NVM 安装 Node.js 完整指南
published: 2026-06-16
description: 详细介绍如何使用 NVM 管理 Node.js 版本，包括 NVM 源配置、版本切换、NPM 源管理以及全局目录配置等实用技巧。
image: api
tags: [Node.js, NVM, NPM, 开发环境]
category: Web
draft: false
---

# NVM 安装 Node.js 完整指南

## 前言

在开发过程中，不同的项目可能需要不同版本的 Node.js。使用 NVM（Node Version Manager）可以方便地在同一台机器上管理多个 Node.js 版本，实现快速切换。本文将详细介绍 NVM 在 Windows 系统下的安装、配置和使用方法。

## 一、NVM 安装与配置

### 1.1 下载安装 NVM

Windows 用户需要下载 [nvm-windows](https://github.com/coreybutler/nvm-windows/releases)，选择最新的 `nvm-setup.exe` 进行安装。

:::NOTE
安装前请先卸载已安装的 Node.js，避免冲突。
:::

### 1.2 更改 NVM 源

由于网络原因，国内用户需要更改 NVM 的下载源。找到 NVM 安装目录下的 `settings.txt` 文件（默认路径：`C:\Users\用户名\AppData\Roaming\nvm\settings.txt`），在文件末尾添加以下内容：

```txt
node_mirror: https://npmmirror.com/mirrors/node/
npm_mirror: https://npmmirror.com/mirrors/npm/
```

或者在命令行中使用以下命令设置：

```powershell
nvm node_mirror https://npmmirror.com/mirrors/node/
nvm npm_mirror https://npmmirror.com/mirrors/npm/
```

## 二、NVM 基本使用

### 2.1 查询可安装的 Node.js 版本

```powershell
# 查看本地已安装的版本
nvm list

# 查看所有可安装的 Node.js 版本
nvm list available

# 查看指定大版本的所有可用版本（如查看 Node.js 18 的所有版本）
nvm list available 18
```

### 2.2 安装 Node.js

```powershell
# 安装最新 LTS 版本
nvm install lts

# 安装最新版本
nvm install latest

# 安装指定版本
nvm install 18.19.0
nvm install 20.11.0
```

### 2.3 切换 Node.js 版本

```powershell
# 切换到指定版本
nvm use 18.19.0

# 切换到已安装的最新版本
nvm use latest

# 查看当前使用的版本
nvm current
```

:::TIP
切换版本时需要以管理员身份运行命令提示符或 PowerShell，否则可能切换失败。
:::

### 2.4 卸载 Node.js 版本

```powershell
# 卸载指定版本
nvm uninstall 18.19.0
```

## 三、NPM 源管理工具 NRM

### 3.1 安装 NRM

NRM（NPM Registry Manager）是一个 NPM 源管理工具，可以快速切换不同的源。

```powershell
# 全局安装 NRM
npm install -g nrm
```

### 3.2 NRM 常用命令

```powershell
# 查看所有可用的源
nrm ls

# 测试所有源的响应时间
nrm test

# 切换到指定源（如切换到淘宝源）
nrm use taobao

# 切换到官方源
nrm use npm

# 添加自定义源
nrm add company http://npm.company.com/

# 删除自定义源
nrm del company

# 查看当前使用的源
nrm current
```

### 3.3 常用 NPM 源列表

| 源名称 | 源地址 |
| ------ | ------ |
| npm | https://registry.npmjs.org/ |
| taobao | https://registry.npmmirror.com/ |
| tencent | https://mirrors.cloud.tencent.com/npm/ |
| huawei | https://repo.huaweicloud.com/repository/npm/ |

## 四、配置 NPM 全局目录

### 4.1 为什么需要配置全局目录

默认情况下，NPM 会将全局包安装在系统盘（通常是 `C:\Users\用户名\AppData\Roaming\npm`），这会占用系统盘空间。通过配置自定义全局目录，可以：
- 节省系统盘空间
- 便于管理和备份
- 避免重装系统后丢失全局包

### 4.2 创建目录结构

首先创建用于存储全局包和缓存的目录：

```powershell
# 创建全局包目录
mkdir D:\Develop\nodejs\node_global

# 创建缓存目录
mkdir D:\Develop\nodejs\node_cache
```

### 4.3 配置 NPM 全局目录和缓存目录

```powershell
# 设置全局包安装目录
npm config set prefix "D:\Develop\nodejs\node_global"

# 设置缓存目录
npm config set cache "D:\Develop\nodejs\node_cache"
```

### 4.4 配置环境变量

配置完成后，需要将全局目录添加到系统环境变量中：

1. **打开系统环境变量设置**
   - 右键"此电脑" → "属性" → "高级系统设置" → "环境变量"

2. **添加系统变量**
   - 在"系统变量"中新建：
     - 变量名：`NODE_PATH`
     - 变量值：`D:\Develop\nodejs\node_global\node_modules`

3. **修改 Path 变量**
   - 在"系统变量"的 `Path` 中添加：`D:\Develop\nodejs\node_global`

:::WARNING
配置环境变量后，需要重启命令行窗口才能生效。
:::

### 4.5 验证配置

```powershell
# 查看当前配置
npm config list

# 查看全局目录
npm config get prefix

# 查看缓存目录
npm config get cache

# 测试全局安装
npm install -g cnpm

# 查看全局安装的包
npm list -g --depth=0
```

## 五、常见问题与注意事项

### 5.1 NVM 切换版本失败

**问题**：执行 `nvm use` 时提示权限不足

**解决方案**：以管理员身份运行命令提示符或 PowerShell

### 5.2 全局安装的命令无法使用

**问题**：全局安装的包（如 `cnpm`、`nrm`）无法在命令行中使用

**解决方案**：
1. 检查环境变量是否正确配置
2. 确认 `D:\Develop\nodejs\node_global` 已添加到 `Path` 中
3. 重启命令行窗口

### 5.3 NPM 安装速度慢

**解决方案**：使用 NRM 切换到国内镜像源

```powershell
# 安装 NRM
npm install -g nrm

# 切换到淘宝源
nrm use taobao
```

### 5.4 不同 Node.js 版本的全局包

:::IMPORTANT
每个 Node.js 版本都有独立的全局包目录。切换 Node.js 版本后，之前版本安装的全局包将不可用，需要重新安装。
:::

### 5.5 查看配置文件位置

NPM 配置文件位于用户目录下的 `.npmrc` 文件中：

```powershell
# 查看配置文件路径
npm config get userconfig

# 通常位于：C:\Users\用户名\.npmrc
```

## 六、总结

通过本文的学习，你应该已经掌握了：

1. **NVM 的安装与配置**：包括更改下载源以加速下载
2. **NVM 的基本使用**：查询、安装、切换和卸载 Node.js 版本
3. **NRM 源管理工具**：快速切换 NPM 源，提升包下载速度
4. **NPM 全局目录配置**：自定义全局包和缓存位置，优化磁盘空间使用

合理配置开发环境可以大大提高开发效率，避免因版本冲突或网络问题导致的困扰。建议在配置完成后，将相关配置记录下来，方便日后查阅或在新环境中快速部署。