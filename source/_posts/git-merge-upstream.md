title: git merge 操作

date: 2020-06-23 19:33:28

categories:
- git的使用

tags:
- git
---

## git merge upstream

```

git remote add upstream https://github.com/OpenAPITools/openapi-generator

git fetch upstream
git checkout master
git merge upstream/master

git push origin master

```

<!--more-->

## git branch merge

```

git branch -a  #先查看下当前的本地和远程分支

git checkout -b my_dev origin/dev  #或者是切换到本地的my_dev分支，假如已经存在的话，即git checkout my_dev 

git pull #将本地分支my_dev对应的远程分支dev拉下来

git checkout master #切换到master分支

git pull #确保master分支也是最新的

git merge my_dev #执行合并的关键代码，此时执行结果时将本地的my_dev合并到本地master分支

git push origin master #将合并的本地master分支推送到远程master

```

## 删除分支

### 删除本地分支

```shell
git branch -d <BranchName>
```

### 删除远程分支

```shell
git push origin --delete <BranchName>
```

### 查看本地分支的追踪情况

```shell
git remote show origin
```

### 删除已经被远程删除的分支

```shell
git remote prune origin

```

## 拉取远程存在，但是本地没有的分支

```shell
git pull

git checkout -b abranch origin/abranch
```

## 解决冲突

解决冲突有两种方法，merge和rebase

### rebase

在开发分支上，执行
```shell
git rebase master
```
解决冲突
执行
```shell
git add .
git rebase --continue
git commit
```

### merge
我自己只用rebase

## 代码回滚

### 方式一，使用revert
会产生新的提交记录

```shell
git revert HEAD
git push origin master
```

### 方式二，使用reset
不会产生新的提交记录

```shell
git reset --hard HEAD^
git push origin master -f
```
