---
title: node包管理工具：nvm-windows
date: 2022-10-21 14:51:38
tags: node
categories: node
description: '一个node包管理工具nvm-windows(nvm的windows版本)' 
---

### 因为新项目使用vue3 + vite 所以将本地的node版本升级了，后来维护老项目的时候node-sass就报错，node-sass与最新的node版本不兼容，这个时候最简单的操作就是将node降级就行了，但是又要开发新项目，所以需要在几个node版本之间来回切换，这个时候就需要一个node版本管理工具，这里我选择了nvm-windows
### nvm-windows[https://github.com/coreybutler/nvm-windows/releases]是windows版本的node管理工具
####  1.下载
###### 1.1 进入github，选择nvm-setup.zip。
![](https://s3.bmp.ovh/imgs/2022/10/21/42f4ccefb2833173.png)
###### 下载解压后是一个安装程序，直接点开安装（安装路径不能有空格和中文路径）
###### 注意：如果windows上本来有node需要先卸载
##### 2. 基本命令使用
+ nvm list available 查看可用版本
![](https://s3.bmp.ovh/imgs/2022/10/21/de9df0509e9e1aa9.png)
+ nvm install <版本号>   安装需要的版本号,如（nvm install 14.94.1）
+ nvm ls  查看已安装的node版本号，正在使用的版本前面会有*
![](https://s3.bmp.ovh/imgs/2022/10/21/4a34e915d2478f88.png)
+ nvm use <版本号> 设置当前使用node的版本号，如（nvm use 14.91.1） 注意：如果设置不成功，那么用管理员身份打开终端再进行设置
