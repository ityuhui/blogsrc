title: 泛型和多态

date: 2023-09-08 21:47:48

categories:

- 编译器和解释器

tags:

- 泛型
- 多态
- generic

---

都可以实现多类型

## 泛型

- 容器内的元素：类型可以参数化
- 模版函数：形参可以参数化，不同的类型可以共享相同的函数

好处是：编译器可以检查类型
一旦类型确定，就不能再更改，所以不能用于容纳多个类型的容器

## 继承多态

不同的派生类型有各自的同名函数

容器可以同时容纳同一个基类的不同的派生类型