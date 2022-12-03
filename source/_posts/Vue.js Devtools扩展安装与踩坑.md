---
title: Vue.js Devtools扩展安装与踩坑
date: 2022-05-09 10:49
layout: archives
description: "自己编译一次Vue.js Devtools工具"
tags: vue Devtools
categories: 前端 
---
#### 一、前言
##### 因为最近练习Vue3项目的时候，发现Chrome浏览器的devtools插件不起作用了，这才想起当前安装的devtools是5版本的，而Vue3项目需要6版本才支持。
****
#### 二、安装
##### 1. 在Github上搜索**devtools**项目，[直达车](https://github.com/vuejs/devtools)
![](https://s3.bmp.ovh/imgs/2022/05/08/3fd173ea052a1fe7.jpg)
##### 2. 找到**devtools**项目**tags**的6.0版本以上的的**beta**版本，下载到本地
![](https://s3.bmp.ovh/imgs/2022/05/08/10f6a897cd289f5e.jpg)
##### 3. 解压后，用编辑器打开，因为这个项目是用**yarn**管理的，而我本地没有安装**yarn**，因此需要安装**yarn**工具,如果不了解yarn的推荐看[这里](https://blog.csdn.net/yw00yw/article/details/81354533?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522165206155416781483777869%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=165206155416781483777869&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-1-81354533-null-null.142^v9^pc_search_result_cache,157^v4^control&utm_term=yarn&spm=1018.2226.3001.4187)
`npm install -g yarn`
##### 4. 执行`yarn install`下载项目依赖，这里如果不成功的话，推荐使用淘宝镜像
##### 5. 执行`yarn run build`打包项目，但是这里报错了
![](https://s3.bmp.ovh/imgs/2022/05/08/b36d114564f3665a.jpg)
###### 这是因为我用的windows，识别不了**rm**，在windows环境下要是用**rimraf**
###### 因此需要安装rimraf,参考[npm包--rimraf：丫丫0721的博客](https://blog.csdn.net/u013530539/article/details/79073799?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522165206215016782388047283%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=165206215016782388047283&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-2-79073799-null-null.142^v9^pc_search_result_cache,157^v4^control&utm_term=rimraf%E5%AE%89%E8%A3%85&spm=1018.2226.3001.4187)
`npm install rimraf --save-dev`
##### 安装完成后，需要把所有的`rm -rf`替换成`rimraf`,只有两个文件夹有，一个就是我们需要的packages\shell-chrome\package.json，另一个在packages\shell-electron\package.json
##### 修改完成后在执行`yarn run build`,就可以打包成功了，这个时候packages\shell-chrome文件夹里面会有一个**build**文件件，这就是我们打包好的文件
#####6. 打开Chrome浏览器的**扩展程序**，点击**加载已解压的扩展程序**，然后找到刚才的shell-chrome文件夹并选择它就可以了。
![](https://s3.bmp.ovh/imgs/2022/05/08/840a0862c72f1f41.jpg)
***
#### 三、总结
##### 因为以前Vue2的时候使用的[扩展迷](https://www.extfans.com/),这是个很好用的插件网站，要关注微信公众号才能下载，并且还有使用教程。这次我主要是想尝试一下自己打包编译一下devtools工具，本以为会很顺利，但没想到还是踩坑了，并且在**rm -rf**这个错误上卡了很久，这里非常感谢[莫得感情学习机1号的博客：踩坑记6 vue3、生命周期钩子、vue-devtools beta](https://blog.csdn.net/Alloom/article/details/119184917)。
