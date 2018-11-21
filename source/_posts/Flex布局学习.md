---
title: Flex布局学习
date: 2018-11-01 17:00
tags:
- Flex
- 布局
toc: true
reward: true
---
Flex布局相关CSS的简单说明
<!-- more -->

### 两根轴线

* 主轴 由 flex-direction 的值确定，值如下

  ```javascript
  flex-direction: {
    row             // 横 左到右 初始值
    row-reverse     // 横 右到左
    column          // 竖 上到下
    column-reverse  // 竖 下到上
  }
  ```

* 交叉轴 与主轴垂直

### Flex 容器

采用了flexbox的区域就是flex容器。把一个容器的`display`属性值改为`flex`或`inline-flex`，容器中的直系子元素就会变成flex元素，所有flex元素都有以下一个初始值：

* 元素排列为一行 (flex-direction 属性的初始值是 row)。
* 元素从主轴的起始线开始。
* 元素不会在主维度方向拉伸，但是可以缩小。
* 元素被拉伸来填充交叉轴大小。
* flex-basis 属性为 auto。
* flex-wrap 属性为 nowrap。

这会让你的元素呈线形排列，并且把自己的大小作为主轴上的大小。如果有太多元素超出容器，它们会溢出而不会换行。如果一些元素比其他元素高，那么元素会沿交叉轴被拉伸来填满它的大小。

所以应该把每一行看作一个新的flex容器，任何空间分布都将在该行上发生，而不影响该空间分布的其他行。

### flex-wrap

可用flex-wrap实现多行Flex容器
cross-start 会根据 flex-direction 的值选择等于start 或before。cross-end 为确定的 cross-start 的另一端

```javascript
  flex-wrap: {
    nowrap          // 初始值，挤在一行，可能导致溢出
    wrap            // lex 元素被打断到多个行中,
    wrap-reverse    // 与wrap的行为一样，cross-start和cross-end互换。
  }
```

### flex-direction 与 flex-wrap 简写成 flex-flow

`flex-flow: <flex-direction> <flex-wrap>`
  
### flex -> flex-grow flex-shrink flex-basis

flex规定了弹性元素如何伸长或缩短以适应flex容器中的可用空间。这是一个简写属性，用来设置flex-grow, flex-shrink与flex-basis。

```javascript
flex: {
  auto      -> flex: 1 1 auto
  initial   -> flex: 0 1 auto
  none      -> flex: 0 0 auto
  <positive-number>   -> flex: <positive-number> 1 0
}
```

* flex-grow 属性定义弹性盒子项（flex item）的拉伸因子
* flex-shrink 属性指定了 flex 元素的收缩规则.flex 元素仅在默认宽度之和大于容器的时候才会发生收缩，其收缩的大小是依据 flex-shrink 的值
* flex-basis 指定了 flex 元素在主轴方向上的初始大小

#### 元素间的对齐和空间分配

* align-items 属性可以使元素在交叉轴方向对齐

  ```javascript
  align-items: {
    stretch   // 默认值
    flex-start
    flex-end
    center
  }
  ```

* justify-content属性用来使元素在主轴方向上对齐

  ```javascript
  justify-content: {
    stretch
    flex-start
    flex-end
    center
    space-around
    space-between
  }
  ```