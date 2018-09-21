---
title: React之JSX
date: 2018-09-20
categories:  
- React
tags:
- jsx
- 组件开发
---

​	

​	JSX语法JSX 是Facebook为React开发的一套语法糖，创建 JSX 语法的本质目的是为了使用基于 xml 的方式表达组件的嵌套，保持和 HTML 一致的结构，语法上除了在描述组件上比较特别以外，其它和普通的 Javascript 没有区别。 并且最终所有的 JSX 都会编译为原生 Javascript。

中文网参考：<https://doc.react-china.org/docs/jsx-in-depth.html> (一定要去看一下)

## 基础 ##

1. 基本规则

   JSX 构建组件的规则和 XML 规则一致 

2. 嵌套规则

   标签可以任意嵌套。例如： 

   ```
   function render(){
       return <p>
       		text content
       		<ul>
       			<li>.......</li>
       			<li>.......</li>
       		</ul>
       	  </p>
   }
   ```

    

3. 标签闭合

   标签必须严格闭合，否则无法编译通过。 

   自闭合： 

   ```
   function render(){
       return <input type="text" />
   }
   ```

   标签闭合： 

   ```
   function render(){
       return <p>.......</p>
   }
   ```

   

4. JSX组件

   JSX 组件分为 HTML 组件和 React 组件，HTML 组件就是 HTML 中的原生标签例如： 

   ```
   function render(){
       return <P>hello,React World</p>
   }
   
   function render(){
       return <ul>
                   <li>list item1</li>
                   <li>list item2</li>
       	  </ul>
   }
   ```

   React 组件就是自定义的组件，例如： 

   ```
   //定义一个自定义组件
   var CustomComponnet = React.createClass({
       render: function() {
           return <div>custom component</div>
       }
   })；
   //使用自定义组件
   function render(){
       return <p> <CustomComponnet /> </p>
   }
   ```

   

5. 组建属性

   和 HTML 一样，JSX 中组件也有属性，传递属性的方式也相同，对于 HTML 组件： 

   ```
   function render(){
       return <p title="title">hello,React ,world</p>
   }
   ```

   如果是 React 组件可以定义自定义属性（Props），传递自定义属性的方式： 

   ```{
   function render(){
       return <p>  <CustomComponnet customProps="data" /> </p>
   }
   ```

   属性即可以是字符串，也可以是任意的 Javascript 变量, 传递方式是将变量用花括号：

   ```
   function render(){
       var data = {a: 1,b:2};
       return <p> <CustomComponnet customProps={data}  </p>
   }
   ```

   **注意：属性的写法上和 HTML 存在区别，在写 JSX 的时候，所有的属性都是驼峰式（Webkit, ms）的写法:** 

   ```
   function render(){
       return <div className="....">
       			<lable htmlFor="..."></lable>
       			<input onChange="...." />
       	   </div>
   }
   ```

   **而原生的写法为:** 

   ```
   <div class="....">
       <lable for="..."></lable>
       <input onchange="...." />
   </div>
   ```

   ​	主要是出于标准的原因，驼峰式是 Javascript 的标准写法，并且 React 底层是将属性直接对应到原生 DOM 的属性，而原生 DOM 的属性是驼峰式的写法，这里也可以理解为什么类名不是 class 而是 className 了, 又因为 class 和 for 还是 js 关键字，所以 jsx 中:   

   ​	class  =>  calssName  

   ​	 for  =>  htmlFor      

   ​	 除此之外比较特殊的地方是 `data-*` 和 `aria-*` 两类属性是和 HTML 一致的。 

6. 显示文本

   很多情况，我们需要将 JS 中的文本直接显示，做法和显示变量属性一样，用花括号 

   ```
   function render(){
       var text = "Hello ,World";
       return <p> {text} </p>
   }
   ```

   

7. 运算

   花括号里边实际上除了变量以外，可以是一段 JS 表达式，我们可以利用花括号做简单的运算： 

   ```
   function render(){
       var text = text;
       var isTrue = false;
       var arr = [1,2,3];
       return <p>
       		{text}
       		{isTrue ? "true" : "false" }
       		{ arr.map(function(it){
                   return <span> {it} </span>
       		})}
       	  </p>
   }
   ```

   

8. 注释

   注释写法如下： 

   ```
   function render(){
       return <p>
       		/* 这里是注释 */
              </p>
   }
   ```

   

9. 限制规则

   render 方法返回的组件必须是有且只有一个根组件，错误情况的例子 

   ```
   //无法编译通过，JSX会提示编译错误
   function render(){
       renter (
       		<p>..........</p>
       		<p>..........</p>
       		)
   }
   ```

   

10. 组件命名空间

   JSX 可以通过命名空间的方式使用组件, 通过命名空间的方式可以解决相同名称不同用途组件冲突的问题。 

   ```
   function render(){
       return <p>
       	  		<CustomComponnet1.SubElement />
       	  		<CustomComponnet2.SubElement />
       	  </p>
   }
   ```

   ​	