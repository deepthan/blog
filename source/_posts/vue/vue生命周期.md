---
title: vue生命周期
date: 2019-01-22 21:20:43
categories: vue
---

```
beforeCreate
```
- 这一步之前当前组件对应对象会被实例化(new vue)，但是不传入数据不会进行创建组件。
- 组件没有被创建所以它的`data`为`undefined`

```
created
```
- 往实例化的对象传入数据创建组件，组件创建完成
- 组件中已传入数据，并且创建成了完整的组件

```
beforeMount
```
- 组件虽然被创建完成了，但是没有放在`dom`元素上，所以不显示

```
mounted
```
组件被放在`dom`元素上并且已渲染完成。

```
beforeUpdate
```
组件被更新之前
```
updated
```
组件被更新之后

```
beforeDestroy
```
组件销毁之前，很多数据都还存在，`dom`元素上还有它。

```
destroyed
```
组件销毁之后，在`dom`上移除了此组件并且此组件已完全销毁，如果要用只能再次重新创建。