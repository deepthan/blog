---
layout: vue
title: vue watch用法
date: 2019-01-22 21:31:47
tags: vue
categories:
- vue
---
> watch 是一个对象，它有键，有值

- 键：你要监控的东西

- 值：可以是三个内容
    - 函数： 当你监控的东西发生变化执行的函数，它有两个参数，一个是变化后的值，一个是旧的值。
    - 或 函数名，它得用单引号包裹。
    - 或 选项的对象
        - handler： 其值是一个回调函数，即监听到对象属性值变化时执行的函数。 
        - deep: 布尔值。是否深度监听。一般可以监听到数组值的变化，但不能监听到对象属性值的变化。
        - immediate: 布尔值，是否以初始值作为新值执行`handler`的函数，老值则会显示为`undefined`
    

    
`deep`是用来监听对象属性的变化，即：
```ts
var vm = new Vue({
  data: {
    b: 'xx',
    c: 'xx',
    obj: {
        a: 'xx'
    }
  }
})

```
上面`obj`对象的`a`属性，需要添加`deep`来监听

##### 1. 普通的`watch`

```ts
data() {
    return {
        demo: 0    
    }
},
watch: {
    demo(newValue, oldValue) {
        console.log(newValue)
    }
}
```

##### 2. 数组的`watch`

```ts
data() {
    return {
        demo: new Array(11).fill(0)   
    }
},
watch: {
　　demo: {
　　　　handler(n, o) {
　　　　　　for (let i = 0; i < n.length; i++) {
　　　　　　　　if (o[i] != n[i]) {
　　　　　　　　　　console.log(n)
　　　　　　　　}
　　　　　　}
　　　　},
　　　　deep: true
　　}
}
```
##### 3. 对象的`watch`
只要bet中的属性发生变化（可被监测到的），便会执行handler函数
```ts
data() {
　　return {
　　　　dog: {
　　　　　　name: 'bob',
　　　　　　age: 13
　　　　}   
    }
},
watch: {
　　dog: {
　　　　handler(n, o) {
　　　　　　console.log(n)
　　　　},
　　　　deep: true
　　}
} 
```
##### 4. 对象具体属性的`watch`

如果想监测具体的属性变化，如`age`变化时，才执行handler函数，则可以利用计算属性computed做中间层。

```ts
data() {
　　return {
　　　  dog: {
　　　　　　name: 'bob',
　　　　　　age: 13
　　　　}   
    }
},
computed: {
　　age: function() {
　　　　return this .dog.age
　　}
},
watch: {
　　age(n, o) {
　　　　console.log(n) 　　
　　    
　　}
}
```

