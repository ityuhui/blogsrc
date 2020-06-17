title: GraphQL介绍

date: 2020-06-17 20:54:49

categories:
- Web开发

tags:
- web
- GraphQL
- REST

---

## 什么是GraphQL

GraphQL 一种用于API的查询语言，具有优于RESTful的特点。它可以只用一个请求获取多个资源。

<!--more-->

## 举例

Request
```GraphQL
{
    hero {
        name
        height
        mass
    }
}
```

Response
```GraphQL
{
    "hero":{
        "name": "Luke Skywalker",
        "height" : 77
    }
}
```