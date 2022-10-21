---
layout: archives
title: Map和Set
date: 2022-05-30 11:39
description: "最近在项目中用到了Set，顺便复习了一下Set的基本API，因为Map和Set很多地方是相通的，所以顺便把Map也带上，本文只介绍基本API "
tags: js ES6
categories: 前端
---
###  最近在项目中用到了Set，顺便复习了一下Set的基本API，因为Map和Set很多地方是相通的，所以顺便把Map也带上，本文只介绍基本API，这两个对象在我在项目中用的很少，至于用它们实现私有方法和存储DOM的引用推荐去看阮一峰的ES6入门，这里推荐一个[大佬博客](https://www.zhangxinxu.com/wordpress/2021/08/js-weakmap-es6/),这篇文章讲WeakMap讲的很好。
------------------
### 1.Map
##### Map是一种新的集合类型,与Object类型相似，它为js带来了真正的键/值存储机制，够保将 <font color="green">任意类型</font> 的值以存键/值对的形式按顺序保存。
##### 在Map中键值是否相等采用的是类似于严格相等(=====)方式来判断，但值得注意的是，在Map中认为NaN与NaN是相等的，尽管实际上NaN !== NaN。
#### Map与Object的差异，以下表格内容来自[此处](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map)
-------------
|                |  Map                                                                       |  Object                                                                                   |
| -----------    | ----                                                                       | ----                                                                                      |
|   键的类型      |   可以是任意值，包括函数、对象或任意基本类型                                  |  必须是一个 String 或是 Symbol类型                                                          |
|   键的顺序      |   Map 中的 key 是有序的。因此，当迭代的时候，一个 Map 对象以插入的顺序返回键值  |  虽然 Object 的键目前是有序的，但并不总是这样，而且这个顺序是复杂的。因此，最好不要依赖属性的顺序。|
|   意外的键      |  Map 默认情况不包含任何键。只包含显式插入的键。                               | 一个 Object 有一个原型, 原型链上的键名有可能和你自己在对象上的设置的键名产生冲突。                |                           
|   集合大小      |  可以通过size属性直接获取                                                   |  Object 的键值对个数只能手动计算.                                                             | 
|   迭代          |  Map 是 可迭代的 的，所以可以直接被迭代                                      |  Object 不能直接迭代                                                                         |
|   性能          |  在频繁增删键值对的场景下表现更好。                                          | 在频繁添加和删除键值对的场景下未作出优化。                                                      |
|   序列化和解析   | 没有元素的序列化和解析的支持。                                              | 原生的由 Object 到 JSON 的序列化支持，使用 JSON.stringify()                                    |

----------------

##### 创建Map集合映射
###### 在初始化时，传入一个可迭代的对象，并且包含键值对的数组，数组的第一个值是键，第二个值是值，这个可迭代的数组对象每一组键值对都会按顺序添加到新映射中。
```
const Map1 = new Map([['a', 1], ['b', 2]])
console.log(Map1); //Map(2) {'a' => 1, 'b' => 2}
```
#### Map的api
+ Map.prototype.set(key,value)   
###### Map的set()方法接收一个键值对,如果该键以存在，则更新该键的值，若不存在，则在Map对象的最后添加一个新的键值对映射，返回一个映射实例，因此可以多个set()链式添加。
```
 Map1.set('key1', 'value1').set('key2', 'value2')
 console.log(Map1)  //Map(4) {'a' => 1, 'b' => 2, 'key1' => 'value1', 'key2' => 'value2'}
```
+ Map.prototype.has(key)
###### Map的has()方法接收一个键名,传入要查询的键，返回一个boolean值 ，存在返回true，不存在返回false,用于查询Map集合中是否存在要查询的映射。
```
console.log(Map1.has('a'));  //true
console.log(Map1.has('c'));  //false
```
+ Map.prototype.get(key)
###### Map的get()方法接收一个键名,传入要查询的键，若集合中存在该建，则返回改建所映射的值，否则返回undefine,用于查询Map集合中是否存在要查询的映射和获取要查询的映射。
```
console.log(Map1.get('a')); // 1
console.log(Map1.get('c')); // undefined
```
+ Map.prototype.size
###### Map的size属性用于获取该集合的大小，返回一个Number类型的值
```
console.log(Map1.size); //4
```
+ Map.prototype.delete(key)
###### Map的delete()方法传入要删除的键，返回值是一个boolean值，如果Map中存在要删除的键，返回true，并表示删除成功，不存在要删除的键，则返回false，表示删除失败。
```
console.log(Map1.delete('c'));    //false
console.log(Map1.delete('a'));    //true
console.log(Map1);  // {'b' => 2, 'key1' => 'value1', 'key2' => 'value2'}
```
+ Map.prototype.clear()
###### Map的clear()方法没有参数，没有返回值，调用此方法可以清空Map中所有的键值对
```
Map1.clear()
console.log(Map1)  //Map(0) {size: 0}
```
##### **<font color="red">注意</font>**：当引用类型作为Map的键或者值的话，我们修改引用的键或值，Map中的映射关系不会改变,
```
const Map2 = new Map()
const key1Obj = { key1: 'key1' }
const value1Obj = { value1: 'value1' }
const keyArr = ['key1']
const ValueArr = ['value1']
Map2.set(key1Obj, value1Obj).set(keyArr, ValueArr)
console.log(Map2);  // {{key1: "key1"} => {value1: 'value1'}, ['key1']=> ['value1']}
key1Obj.key2 = 'key2'
ValueArr.push('value2')
console.log(Map2); // {{key1: "key1",key2:"key2"} => {value1: 'value1'}, ['key1']=> ['value1','value2']}
```
#### Map的迭代方式
###### Map映射实例可以提供一个迭代器（Iterator）能以插入顺序生成[key, value]形式的数组，因此Map是可以遍历的。可以通过 entries()方法（或者 Symbol.iterator 属性）取得这个迭代器。
###### Map.prototype.entries() === Map.prototype[@@iterator]()
+  for... of 
```
const Map3 = new Map([['key1', 'value1'], ['key2', 'value2'], ['key3', 'value3']])

for (let el of Map3.entries()) {
  console.log(el);  // ['key1', 'value1']  ['key2', 'value2']  ['key2', 'value2']
}
for (let el of Map3[Symbol.iterator]()) {
  console.log(el);  // ['key1', 'value1']  ['key2', 'value2']  ['key2', 'value2']
}
```
###### Map.prototype.forEach(callback(value,key,map),thisArg)  
+ Map.forEach()接受两个参数，第一个参数是一个回调函数，该回调函数接收三个可选值，第一个值value是每个迭代的值，第一个值key是每个迭代的键，第三个值map是要迭代的Map对象。第二个参数thisArg用于指定回调函数中this的值
```
Map3.forEach((value, key) => {
   console.log(key);       //key1 ，key2，key3
   console.log(value);    //value1 ，value2 ，value3
})
```
##### 如果只想遍历Map的值或键，可以调用values()或keys()方法，这两个方法分别返回以插入顺序生成值或键的迭代器
```
for (let key of Map3.keys()) {
  console.log(key);  //kye1 key2 key3
}
for (let key of Map3.values()) {
  console.log(key);  //value1 value2 value3
}
```
#### **<font color="red">注意</font>**：如果迭代的键或者值不是引用类型，那么在迭代过程中是不可修改的，如果是引用类型的值或者键，修改后映射关系不会改变
```
      const Map4 = new Map([['key1', 'value1']])
      for (let key of Map4.keys()) {
        key = 'newKey'
        console.log(Map4.get('newKey'));  //undefined
        console.log(Map4.get('key1'));  //value1  
       }

     const keyObj = { a: '1' }
     const valueObj = { value: 'val' }
     const Map5 = new Map([
       [keyObj, valueObj]
     ])
     for (let key of Map5.keys()) {
       key.a = '2'
       key.b = '3'
       console.log(key);  //{a:'2',b:'3' }
       console.log(Map5.get(keyObj));  //{ value: 'val' }
     }
     console.log(Map5);  // {a:'2',b:'3' } => { value: 'val' }
     for (let value of Map5.values()) {
       value.value = '2'
       value.newVal = 'newVal'
       console.log(Map5.get(keyObj));  //{value: '2', newVal: 'newVal'}  修改成功
     }
     console.log(Map5);  //{a: '2', b: '3'} =>{value: '2', newVal: 'newVal'}  修改成功
```
### 2.WeakMap

###### WeakMap是Map的“兄弟”类型，其 API 也是 Map 的子集，WeakMap对象是一组键/值对的集合，其中的键是弱引用的。<font color= "green">其键必须是对象，而值可以是任意的</font>。
#### WeakMap的API
+ WeakMap.prototype.set(key,value)
###### WeakMap的ste()方法与Map的set()方法一样，接收一组key，value映射，用于设置/更新WeakMap对象，其中，key只能是对象类型
+ WeakMap.prototype.has(key)
###### WeakMap的has()方法与Map的has()方法一样，接收一个要查询的key，返回一个Boolean值，true表示要查询的映射存在，false表示要查询的映射不存在
```
    const WMap1 = new WeakMap()
    const key1 = {}
    const key2 = {}
    WMap1.set(key1, 'value1')
    console.log(WMap1.has(key1));  //true
    console.log(WMap1.has(key2));  //false
```
+ WeakMap.prototype.get(key)
###### WeakMap的get()方法接收一个要获取的key，返回对应的值，如果要回去的映射不存在，则返回undefined
```
console.log(WMap1.get(key1));  //value1
console.log(WMap1.get(key2));  //undefined
```
+ WeakMap.prototype.delete(key)
###### WeakMap的delete()方法与Map的delete()方法一样，接收一个要删除的key，要删除的映射存在时返回true，并表示删除成功，要删除的映射不存在则返回false
```
console.log(WMap1.delete(key1));  //true 删除成功
console.log(WMap1.delete(key));  //false 因为WMap1中已经不存在key1，所以返回false
```
---
### 3.Set
##### Set和Map很像，都是ECMAScript6新增的一种新的集合类型，大多数API和行为都是共有的，**<font  color="green">Set最大的特点是Set对象允可以存储任何类型唯一值，无论是原始值或者是对象引用，并且会自动去重，使集合里面的值具有唯一性</font>**，用来去重很方便。

##### 创建Set对象,在Set在判断对象子项是否相等是采用的是严格相等，特别注意的是，在Set集合中，NaN被判断为相等
```
     const set1 = new Set(['1', '2', 2, '3', '3', 2])
     console.log(set1);//Set(4) {'1', '2', 2, '3'}  
     const set2 = new Set([null, undefined, null, undefined,NaN,NaN])
     console.log(set2);  //Set(3) {null, undefined,NaN}
```
#### Set的API
+ Set.prototype.add(value)
###### Set的add()接受一个值，并返回集合的实例，因此可以链式调用
```
 const set3 = new Set()
 set3.add(1);
 set3.add(2).add(3)
 console.log(set3); //Set(3) {1, 2, 3}
```
+ Set.prototype.has(value)
###### Set的has()方法用于查询是否有某个元素 返回值是一个boolean值，如果存在返回true，不存在则返回false
```
     console.log(set3.has(3)); // true
     console.log(set3.has(4)); //false
```
+ Set.prototype.size
###### Set的size属性返回Set对象的大小
```
  console.log(set3.size); //  3
```
+ Set.prototype.delete(value)
###### Set的delete()方法接收一个要删除的值，返回一个boolean值，集合中有要删除的value则返回true表示删除成功，没有则返回false表示删除失败
```
    console.log(set3.delete(4)); //false
    console.log(set3.delete(3)); //true
    console.log(set3);     //Set(2) {1, 2}
```
+ Set.prototype.clear()
###### Set的clear()方法用于清空Set对象，没有返回值
```
 set3.clear()
 console.log(set3);  //Set(0) {size: 0}
```
#### Set的迭代方式
##### Set实例也可以提供一个迭代器（Iterator），能以插入顺序生成集合内容。可以通过 values()方法及其别名方法 keys()（或者 Symbol.iterator 属性，它引用 values()）取得这个迭代器：
+ for ... of
```
     const set4 = new Set([1, 2, 3, 4, 5, 6, 7])

     for (const el of set4.values()) {            
       console.log(el);  //1, 2, 3, 4, 5, 6, 7
     }

     for (const el of set4.keys()) {
      console.log(el);  //1, 2, 3, 4, 5, 6, 7
     }

     for (const el of set4[Symbol.iterator]()) {
       console.log(el);  //1, 2, 3, 4, 5, 6, 7
     }
     //以上三种方式等价
     // Set对象还有个 entries()方法，可以按照插入顺序产生包含两个元素的数组，这两个元素是集合中每个值的重复值
     for (const el of set4.entries()) {
       console.log(el);  //[1,1], [2,2], [3,3], [4,4], [5,5],[6,6] , [7,7]
     }
```
+ Set.prototype.forEach(callback(value,key,set),thisArg)
###### forEach()以回调函数方式遍历，接收两个参数，第一个参数是一个回调函数，该回调函数接收三个可选值，第一个值value为当前遍历的值，第二个值为当前遍历的索引，因为Set没有索引，所以，key也表示当前遍历的值，第三个元素为当前遍历的Set对象。第二个参数可以修改回调函数的this指向
```
 set4.forEach((value1, value2) => {
      console.log('value1:' + value1, 'value2:' + value2);
    })
 //value1:1 value2:1
 //value1:2 value2:2
 //value1:3 value2:3
 //value1:4 value2:4
 //value1:5 value2:5
 //value1:6 value2:6
 //value1:7 value2:7
```
##### 注意 ：如果迭代值是值类型，那么在迭代过程中原来对象的值不会可修改，如果是引用类型的值则可以修改，这点和Map一样

### 4.WeakSet
######  WeakSet是Set 的“兄弟”类型，其 API 也是 Set 的子集。WeakSet 中的“weak”（弱），描述的是 JavaScript 垃圾回收程序对待“弱集合”中值的方式。
##### WeakSet与Set的区别
###### 1.弱集合中的值只能是 Object 或者继承自 Object 的类型 ，不能是其他类型，只要有任意类型不是Object，那么初始化就会失败
###### 2.弱集合对其元素都是弱引用，当其引用的值为空时，就会被销毁触发垃圾回收机制
###### 3.弱集合对其元素随时都可能被销毁，所以不可遍历
##### 创建 WeakSet集合,必须传入可迭代集合,null类型被认为是undefined。
```
const WSet1 = new WeakSet([{ a: '1' }])
```
+ WeakSet.prototype.add(value)
###### WeakSet的add()方法只是传入一个对象，返回一个WeakSet实例，因此 add()方法也可以链式调用，与Set相比，除了插入的值类型有限制以外，其他的都一样
+ WeakSet.prototype.has(value)
###### WeakSet的has()方法与Set的has()方法一样，传入要查询的值，返回一个Boolean值，true表示存在，false表示不存在
```
const WSet2 = new WeakSet()
const value1 = {name:'张三'}
const value2 = {name:'李四'}
WSet2.add(value1 )
WSet2.has(value1 )//true
WSet2.has(value2) //false
```
+ WeakSet.prototype.delete(value)
###### WeakSet的delete()方法与Set的delete()方法一样，传入要删除的值，返回一个Boolean值，true表示要删除的值存在，并成功删除，false表示要删除的值不存在
```
WSet2.delete(value1 )//true   // 删除成功
WSet2.delete(value1) //false  //因为已经删除，所以对象在不存在，再次删除失败
```

