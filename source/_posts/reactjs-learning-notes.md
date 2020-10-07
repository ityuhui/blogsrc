title: Reactjs 学习笔记
date: 2020-10-06 21:48:17
categories:
- 前端开发
tags:
- react
---

## 预备知识：JavaScript新特性

* let,const

定义常量

* class

* 箭头表达式

```javaScript
x => x*x
```
的意思是 
```javaScript
function(x) { 
    return x*x
}
```
箭头表达式没有this

---

<!-- more -->

## JSX

* 大括号{}内是JavaScript表达式

* JSX本身也是JavaScript表达式，可以作为参数传入JS函数，也可以作为JS的返回值。

---

## 组件

### 函数组件

定义组件最简单的方式就是编写 JavaScript 函数：
```JavaScript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```
该函数是一个有效的 React 组件，因为它接收唯一带有数据的 “props”（代表属性）对象与并返回一个 React 元素。这类组件被称为“函数组件”，因为它本质上就是 JavaScript 函数。

### 类组件

```JS
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

上述两个组件在 React 里是等效的。