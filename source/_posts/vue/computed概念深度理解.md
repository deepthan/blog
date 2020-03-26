---
title: vue computed概念深度理解
date: 2019-01-22 21:20:43
categories: vue
---

##  1.1 关于 `computed` 概念理解

官方文档解释为 ==**计算属性**== ，其实解释为 **==属性的计算==** 或许更明显些，
是在这个函数中对 ==**属性**== 进行相关的计算，然后供别的地方使用。它里面的属性是绑定在vue实例中的属性，可以通过 `this.属性名` 进行访问。

关于属性的计算有两种写法：

``` ts
data() {
    return {
        deep: 1
    }
}
```

> 第一种

``` ts

computed(){
    demoProp: function(){
        return this.deep++
    }
}

```

> 第二种

``` ts
computed(){
    demoProp: {
        get: function(){
            return this.deep++
        }
    }
}

```

调用

``` ts
creat(){
    this.demoProp;
}
```

这里直接用 `this.demoProp` 即可访问到 `computed` 里面的 `demoProp` 属性。

## 1.2 性能相关

`computed` 中是计算属性，是属性，而 `metholds` 中的是方法。两种方式的最终结果是完全相同的。  

 不同的是 **计算属性是基于它们的依赖进行缓存的**  
 
 计算属性只有它相关的依赖（变量）发生改变时才重新求值。这就意味着只要 变量还没有发生改变，多次访问属性 计算属性会立即返回之前的计算结果，而不必再次执行函数。  
 
例子：

1. 在 `computed` 中定义一个 `comNow` 的属性，

``` ts
computed: {
    comNow: function(){
        return Data.now()
    }
}
```

2. 在 `metholds` 中定义一个 `metNow` 的方法

``` js
metholds: {
    metNow() {
        return Data.now();
    }
}
```

以上返回的都是其被调用的时间，  

3. 在生命周期 `creat` 中进行调用

``` js
creat() {
    this.getTime();

    let self = this;

    function getTime() {
        window.setInterval(function() {
            console.log("comNow:", this.comNow, "metNow:", this.metNow())
        }, 1000)
    }
}
```

通过在 `getTime` 函数中定时打印出各自的返回值看是否变化。

结果：

``` js
this.comNow 1528440256480 this.metNow 1528440258479
this.comNow 1528440256480 this.metNow 1528440259478
this.comNow 1528440256480 this.metNow 15284402 60479
this.comNow 1528440256480 this.metNow 1528440261479
this.comNow 1528440256480 this.metNow 1528440262478
this.comNow 1528440256480 this.metNow 1528440263479
```

由上结果可以看出：  
在 `computed` 中的属性 `comNow` 不变化，说明它不会重新被获取而是从缓存中直接读取的。  
而 `metholds` 中的 `metNow` 一直在变化，说明其在每次被调用的时候都会重新进行计算，每当触发重新渲染时，调用方法将总会再次执行函数。

    

> 我们为什么需要缓存？假设我们有一个性能开销比较大的计算属性 A，它需要遍历一个巨大的数组并做大量的计算。然后我们可能有其他的计算属性依赖于 A 。如果没有缓存，我们将不可避免的多次执行 A 的 getter！如果你不希望有缓存，请用方法来替代。

总结： == `computed` 依赖缓存，而 `metholds` 却不是。==

