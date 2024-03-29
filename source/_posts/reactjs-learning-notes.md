title: Reactjs 学习笔记
date: 2020-10-06 21:48:17
categories:
- 前端开发 Frontend Development
tags:
- react
- reactjs
- javascript
- jsx
---

## 预备知识：JavaScript新特性

* let

```
定义块作用域变量
```

* const

```
定义本身不可以被再次赋值的常量，但它的所存储的值是可以改变的。
```

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

## 原理

```JavaScript
React.createElement("h1", { id: "recipe-0" }, "Baked Salmon");
```
- 第一个参数是要创建的 http tag
- 第二个参数是它的property
- 第三个参数是它的children，可以是tag

```JavaScript
const dish = React.createElement("h1", null, "Baked Salmon");

ReactDOM.render(dish, document.getElementById("root"));
```
- 第一个参数是渲染的React JavaScript对象
- 第二个参数是要渲染的 html tag

## JSX

* JSX 其实就是替换 `React.createElement` 用的
  ```JavaScript
  React.createElement(Alist, {list:[...]});   // JS
  <Alist list={[...]}> // JSX
  
  ```
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

使用 Hook 来做 函数组建的 state 管理。 

### 类组件(未来将废弃)

```JS
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

上述两个组件在 React 里是等效的。

## Router

使用 reactjs router
https://reactrouter.com/

### 在 Link 之间添加空格

app.css
```css
.navBarLink {
  margin: 5px;
}
```

app.jsx
```js
<Link className='navBarLink' to="/">Home</Link>
```
