title: C++ 易错提醒

date: 2021-08-11 21:40:08

categories:
- C和CPP

tags:
- CPP
---

* 不要返回局部对象的引用
```cpp
std::string & func()
{
    std::string temp = "hello";
    return temp; // Wrong！
}
```
* 可以返回类对象成员的引用，因为在对象存续期间，该成员也是存在的。

* 引用其实是指针的语法糖