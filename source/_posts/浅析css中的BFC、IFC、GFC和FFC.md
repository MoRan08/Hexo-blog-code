---
title: 浅析css中的BFC、IFC、GFC和FFC
date: 2018-09-11 
categories:  
- CSS
tags:
- css3
- 布局
- FC
---


> CSS中各种布局的背后，实质上是各种*FC的组合。CSS2.1 中只有 BFC 和 IFC, CSS3 中还增加了 FFC 和 GFC。 

​	FC的全称是：Formatting Contexts，是W3C CSS2.1规范中的一个概念。它是页面中的一块渲染区域，并且有一套渲染规则，它决定了其子元素将如何定位，以及和其他元素的关系和相互作用。	

​	FC一共包含BFC、IFC、GFC 、FFC四种类型。CSS2.1规范中只有BFC、IFC。CSS3推出GFC、FFC两种新类型。

## BFC

​	BFC(Block Formatting Contexts)直译为"块级格式化上下文"。Block Formatting Contexts就是页面上的一个隔离的渲染区域，容器里面的子元素不会在布局上影响到外面的元素，反之也是如此。

BFC有一下特性：

1. 内部的Box会在垂直方向，从顶部开始一个接一个地放置。
2. Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生叠加
3. 每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。
4. BFC的区域不会与float box叠加。
5. BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之亦然。
6. 计算BFC的高度时，浮动元素也参与计算。

如何产生BFC？

- float的值不为none。 
- overflow的值不为visible。 
- position的值不为relative和static。
- display的值为table-cell, table-caption, inline-block中的任何一个。 

​        那BFC一般有什么用呢？比如常见的多栏布局，结合块级别元素浮动，里面的元素则是在一个相对隔离的环境里运行。

## IFC

​	IFC(Inline Formatting Contexts)直译为"内联格式化上下文"，IFC的line box（线框）高度由其包含行内元素中最高的实际高度计算而来（不受到竖直方向的padding/margin影响)。

IFC有一下特性：

   1.IFC中的line box一般左右都贴紧整个IFC，但是会因为float元素而扰乱。float元素会位于IFC与与line box之间，使得line box宽度缩短。 

   2.IFC中时不可能有块级元素的，当插入块级元素时（如p中插入div）会产生两个匿名块与div分隔开，即产生两个IFC，每个IFC对外表现为块级元素，与div垂直排列。

那么IFC一般有什么用呢？

-  水平居中：当一个块要在环境中水平居中时，设置其为inline-block则会在外层产生IFC，通过text-align则可以使其水平居中。
-  垂直居中：创建一个IFC，用其中一个元素撑开父元素的高度，然后设置其vertical-align:middle，其他行内元素则可以在此父元素下垂直居中。

## GFC

​	GFC(GridLayout Formatting Contexts)直译为"网格布局格式化上下文"，当为一个元素设置display值为grid的时候，此元素将会获得一个独立的渲染区域，我们可以通过在网格容器（grid container）上定义网格定义行（grid definition rows）和网格定义列（grid definition columns）属性各在网格项目（grid item）上定义网格行（grid row）和网格列（grid columns）为每一个网格项目（grid item）定义位置和空间。 

​	GFC将改变传统的布局模式，他将让布局从一维布局变成了二维布局。简单的说，有了GFC之后，布局不再局限于单个维度了。这个时候你要实现类似九宫格，拼图之类的布局效果显得格外的容易。

## FFC

​	FFC(Flex Formatting Contexts)直译为"自适应格式化上下文"，display值为flex或者inline-flex的元素将会生成自适应容器（flex container）。

​	Flex Box 由伸缩容器和伸缩项目组成。通过设置元素的 display 属性为 flex 或 inline-flex 可以得到一个伸缩容器。设置为 flex 的容器被渲染为一个块级元素，而设置为 inline-flex 的容器则渲染为一个行内元素。

​	伸缩容器中的每一个子元素都是一个伸缩项目。伸缩项目可以是任意数量的。伸缩容器外和伸缩项目内的一切元素都不受影响。简单地说，Flexbox 定义了伸缩容器内伸缩项目该如何布局。

整体来说，FFC与BFC有点儿类似，但仍有以下几点区别：

- Flexbox 不支持 ::first-line 和 ::first-letter 这两种伪元素
- vertical-align 对 Flexbox 中的子元素 是没有效果的
- float 和 clear 属性对 Flexbox 中的子元素是没有效果的，也不会使子元素脱离文档流(但是对Flexbox 是有效果的！)
- 多栏布局（column-*） 在 Flexbox 中也是失效的，就是说我们不能使用多栏布局在Flexbox 排列其下的子元素
- Flexbox 下的子元素不会继承父级容器的宽



原文网址：https://yq.aliyun.com/articles/130553

