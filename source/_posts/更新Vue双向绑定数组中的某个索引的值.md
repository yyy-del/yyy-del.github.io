---
title: 更新Vue双向绑定数组中的某个索引的值
date: 2023-03-23 16:36:52
description: "vue2 对象和数组双向绑定失效处理方法"
tags: [vue]
---

### 前言：因为项目需要单独修改表格中某一行(条)数据，不刷新整个表格(表格用的是element-ui的table组件)。

#### 遇到的困难：最开始是直接用数组索引来修改`this.tableData[index] = newVal`,然后发现数据(tableData)是修改了，但是界面上没有响应，即双向绑定失效了，想了很多办法，绑定row-key、使用this.$forceUpdate()强行更新界面都不行(虽然不建议使用this.$forceUpdate())，直接给table组件绑定key来更新是可以的，但是这样直接更新了整改table组件，不优雅，而且每次表格都会默认滑到顶部。

#### 原因：[vue2的文档中明确地写了](https://v2.cn.vuejs.org/v2/guide/reactivity.html)：**由于 JavaScript 的限制，Vue 不能检测数组和对象的变化。尽管如此我们还是有一些办法来回避这些限制并保证它们的响应性。**

#### 以下是官网中的例子

##### 对于对象

```
//Vue 不能检测以下数组的变动：

//利用索引直接设置一个数组项时，例如：vm.items[indexOfItem] = newValue
//修改数组的长度时，例如：vm.items.length = newLength

var vm = new Vue({
  data:{
    a:1
  }
})

// `vm.a` 是响应式的

vm.b = 2
// `vm.b` 是非响应式的

```
##### 对于数组
```
var vm = new Vue({
  data: {
    items: ['a', 'b', 'c']
  }
})
vm.items[1] = 'x' // 不是响应性的
vm.items.length = 2 // 不是响应性的
```
#### 出现上述问题的原因是vue的响应式原理，vue响应式是**将遍历此对象所有的 property，并使用 Object.defineProperty 把这些 property 全部转为 getter/setter**，在上诉对象中vm.a是被添加了getter/setter方法，但是vm.b并没有被添加，而数组items也是被添加了getter/setter方法，但是修改items内部某个索引对应的值时，items的引用并没有被改变，因此并没有触发items的setter，因此双向绑定失败。

### 解决方式
#### vue在文档中已经给出了处理这种双向绑定失效的方法，即`Vue.set( target, propertyName/index, value )`

```
Vue.set( target, propertyName/index, value )
{Object | Array} target
{string | number} propertyName/index
{any} value

//返回值：设置的值

//用法：
//向响应式对象中添加一个 property，并确保这个新 property 同样是响应式的，且触发视图更新。它必须用于向响应式对象上添加新 property，
//因为 Vue 无法探测普通的新增 property (比如 this.myObject.newProperty = 'hi')


//因此：上诉对象和数组的响应式方法是使用set方法来修改，添加响应
Vue.set(vm.someObject, 'b', 2)
Vue.set(vm.items, indexOfItem, newValue)
```
