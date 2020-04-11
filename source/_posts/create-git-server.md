title: 如何创建一个自己的git服务器

date: 2014-01-27 22:24:59

categories:
- git的使用

tags:
- git

---

## 前提条件

客户端：Windows

服务器：Ubuntu

<!--more-->

## 安装

### 客户端的安装

安装git

生成 idrsa, idrsa.pub

```shell
ssh-keygen -t rsa
```

### 服务器的安装和使用

安装git

```shell
apt-get install git-core
```

将客户端的id_rsa.pub里的内容放到.ssh目录下的配置文件里

```shell
cat 客户端的id_rsa_user1.pub >> 服务器的~/.ssh/authorized_keys
```

建立Git Repository

```
mkdir -p /some/dir/project_name.git
cd /some/dir/project_name.git
git init --bare --shared
```

### 客户端的使用

有两种方法

#### 1

```shell
git clone git@example.com:/var/cache/git/project_name.git
cd project_name
vim test.txt
git add .
git commit -m 'add test.txt'
git push origin master
```

#### 2

```shell
mkdir project_name
cd project_name
git init
git add .
git commit -m 'initial commit'
git remote add origin git@example.com:/var/cache/git/project_name.git
git push origin master
```

## 参考文档

http://blog.csdn.net/markddi/article/details/8278015