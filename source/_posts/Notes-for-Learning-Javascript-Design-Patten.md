title: "Notes for Learning Javascript Design Patten"
date: 2015-04-16 04:57:30
tags:

---

## 设计模式的分类：

### 创建型设计模式：
Constructor, Factory, Abstract, Prototype, Singleton and Builder
### 结构型设计模式
Decorator, Facade, Flyweight, Adapter and Proxy
### 行为型设计模式
Iterator, Mediator, Observer and Visitor

Tips:
Javascript是一门没有“类”的语言，但是可以用function来模拟"类"

<!--more-->

## The Constructor Pattern
这是Javascript特有的模式，其实就是传统面向对象编程语言里的类的构造函数

```
var car1=new Car();
function Car(){
  this.toString = function(){
}
```

toString函数会在每个对象中存在一份，所以应该使用prototype

```
Car.prototype.toString = function () {
};
```

