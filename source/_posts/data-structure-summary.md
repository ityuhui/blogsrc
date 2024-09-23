title: 编程语言的数据结构

date: 2024-09-23 17:19:23

categories:

- 软件开发通用知识

tags:

- 数据结构
- data structure
- java
- c++
- c

---

# 编程语言的数据结构

## 1 一般性总结

### 1.1 底层

在数据结构理论里，线性表底层物理实现只有两种：

- 数组 array
- 链表 linked

<!--more-->

### 1.2 高层

#### 1.2.1 最常见的（一般高级一点的编程语言自带实现）

- 线性表 list
- 哈希表 hash

#### 1.2.2 比较常见的

- 栈 stack
- 队列 queue

#### 1.2.3 高级的

- 堆 heap
- 树 tree
- 图 graphic

## 2 各语言自带的数据结构

### 2.1 Java

#### Array

```java
int[] array = new int[5];
```

#### ArrayList

```java
List<String> arrayList = new ArrayList<>();
```

#### LinkedList

```java
List<Integer> linkedList = new LinkedList<>();
```

#### HashMap

```java
Map<String, Integer> hashMap = new HashMap<>();
```

#### treeMap

```java
Map<String, Integer> treeMap = new TreeMap<>();
```

#### 其他更高级的数据结构看参考

<https://www.runoob.com/java/java-data-structures.html>

### 2.2 JavaScript

#### 2.2.1 Array

#### 2.2.2 Object

#### 2.2.3 Set

#### 2.2.4 Map

### 2.3 Python

#### List

```python
cars = ["Toyota", "Tesla", "Hyundai"]
```

#### Dict

```python
my_dict = {'First': 'Python', 'Second': 'Java'} 
```

#### Tuple

类似于 list，但元素不可修改

#### Set

元素不可重复

### 2.4 C++

#### 顺序容器

- vector: 数组实现，单向，相当于Java的 ArrayList
- list： 双向链表，相当于Java的 LinkedList
- deque：双向队列

#### 关联容器，用树实现

- map

#### hash 容器

- unordered_map

#### 容器适配器，使用容器实现

- stack：实现deque实现
- queue：使用deque实现
- priority_queue：使用vector实现

### 2.5 Golang

### 2.6 Rust
