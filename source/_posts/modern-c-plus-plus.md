title: 现代C++

date: 2020-06-16 21:15:40

categories:
- C和CPP

tags:
- CPP
- Modern C++
- C++17

---

## Move语义

## 智能指针的推荐用法

### 1.
不要再使用new, delete, 一律用make_shared,make_unique代替

### 2.
只有具有ownership的关系，才用智能指针，否则使用 T &, 或者 T *

<!--more-->

## 新的数据类型

### std::any
类似于void *

### std::variant
类似于c语言里的union

## Lamda

## 多线程库
