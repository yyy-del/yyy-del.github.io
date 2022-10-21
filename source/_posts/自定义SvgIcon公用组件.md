---
title: 自定义SvgIcon公用组件
date: 2022-08-03 14:32
description: "一个公用svg组件"
category: 前端
tags: vue svg
---
## 前言
### 因为项目中有很多地方需要用到字体svg图标，而项目中使用了很多不同来源的字体图标导致维护很麻烦，每一次需要新添加字体图标的时候就会有很麻烦的操作，因此想开发一个svg组件优化一下字体图标的引入，只需要将要引入svg文件放入指定文件夹中，然后通过svg组件绑定文件名称，就可以达到引用字体图标的目的。
## 1.安装loader
### 要处理svg文件，需要要svg-sprite-loader，解析svg，因为是只在开发时需要解析，因此加上后缀 --save-dev或者-D。
`npm install svg-sprite-loader --save-dev`
## 2.使用svg-sprite-loader
### 在vue.config.js中去除旧的svg规则，然后添加新的svg规则
```
const path = require('path')  //因为引入svg文件时需要获取文件路径，因此需要引入path 
module.exports = { 
  chainWebpack: config => {
       config.module
       .rule('svg')
       .exclude
       .add(path.join(dirname, 'src/assets/icons')) 
       .end()
      config.module
       .rule('icons')
       .test(/.svg$/)
       .include
       .add(path.join(dirname, 'src/assets/icons')) 
       .end() 
       .use('svg-sprite-loader') 
       .loader('svg-sprite-loader')
       .options({ symbolId: 'icon-[name]' })  
       .end()
  }
}
```
## 3.创建SvgIcon组件
### 这个组件将会是一个公用组件，可以在很多项目中使用，因此建议放在公用组件文件夹中
```
//此组件接收一个必传的参数name：svg文件的名称
<template>
    <svg class="svg-icon" aria-hidden="true">
        <use :xlink:href="`#icon-${name}`" />
    </svg>
</template>
<script>
export default {
  name: 'SvgIcon',
  props: {
    name: { // svg文件名称
      type: String,
      required: true
    }
  }
}
</script>
<style scoped>
.svg-icon {
  width: 1em;
  height: 1em;
  vertical-align: -0.15em;
  fill: currentColor;
  overflow: hidden;
}
</style>

```
## 4.注册SvgIcon组件
### 组件注册可以全局注册，也可以局部注册，鉴于有很多组件和页面都要使用它，因此这里选择全局注册
```
// main.js 中   引入SvgIcon组件，全局注册SvgIcon组件
import SvgIcon from '@/base-ui/svgIcon/SvgIcon.vue'
Vue.component('svg-icon', SvgIcon)

```
## 5.引入svg资源 
###  使用require.context(directory,useSubdirectories,regExp) 工具函数将文件夹所有后缀是svg的资源文件引入。
```
// require.context(directory,useSubdirectories,regExp)
// 1.directory :检索目录
//2.useSubdirectories：是否检索该目录下的字文件夹，true：检索，false：不检索
//3.regExp:匹配文件的正则表达式,一般是文件名

```
### require.context(directory,useSubdirectories,regExp)返回一个函数，它的原型上有1个id属性2个方法：
+ 1：id，
+ 2：keys（）这是已个函数，调用可以得到匹配到的文件名数组 
+ 3：resolve（req） 这是一个函数，接受一个参数，返回文件资源的路径
![](https://s3.bmp.ovh/imgs/2022/08/03/6bb2808fd65d6f84.jpg)
```
//main.js

// 1. 引入SvgIcon组件，全局注册Svg
Icon组件
import SvgIcon from '@/base-ui/svgIcon/SvgIcon.vue'
Vue.component('svg-icon', SvgIcon)
// 2. 载入所有svg icon
const requireContext = require.context('./assets/icons', false, /\.svg$/)
requireContext.keys().forEach(requireContext)

```
## 6.使用效果
### 图中svg来自阿里巴巴矢量图,注意在下载的时候记得去掉svg图标的颜色，这样才可以修改图标的颜色。
![](https://s3.bmp.ovh/imgs/2022/08/03/8b70e36202a061c9.jpg)
