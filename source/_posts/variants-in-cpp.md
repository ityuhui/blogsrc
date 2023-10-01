title: C++ 容器怎么存放不同类型的值

date: 2023-10-01 17:28:05

categories:

- C和CPP

tags:

- C++
- C
- 容器
- 类型

---

最简单粗暴的方法是

```c
void *
```

稍微工程一点的方法是

```c
struct MyType{
    enum class TypeName;
    union data;
};
```

再OO一点的方法是

```c
class IObject{};
```

作为通用基类。
