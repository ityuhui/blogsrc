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

### 刷新远程的分支列表
```shell
git remote update origin --prune  #刷新origin
git remote update upstream --prune #刷新upstream
```
可以取代下面这个命令
### 删除已经被远程删除的分支

```shell
git remote prune origin

```

## 拉取远程存在，但是本地没有的分支

```shell

git remote update origin --prune

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

## git fetch and git merge
虽然从效果上看 git pull = git fetch + git mrege/rebase，但是最好用两步的方法

```shelll
git fetch orgin master //将远程仓库的master分支下载到本地当前branch中

git log -p master  ..origin/master //比较本地的master分支和origin/master分支的差别

git merge origin/master //进行合并，注意merge/rebase命令后面跟的是branch，不能直接跟名字，例如origin
```

## 处理不同分支之间的指定文件的差异

### 在不同分支之间比较文件

```
git diff branch1 branch2 filename
git diff branch localfilename
```

### 在不同分支之间合并文件

```
# at branch feature
git checkout release
# at branch release
git checkout feature file-1
git checkout feature file-2
...
git push -f 
```

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

## 撤销本地的修改

### 单个文件
```shell
git checkout -- ${filename}
```

### 所有文件
```shell
git checkout .
```

## 标签tag

### 打一个带annotation的tag
```
git tag -a v0.1.0 -m "version 0.1.0"
```

### 查看tag
```
git tag -l
git show v0.1.0
```

### 上传tag
```
git push origin v0.1.0
```

### 删除本地 tag
```
git tag -d v0.1.0
```

### 删除远程 tag

```shell
git push --delete origin v0.1.0
```



## 单个文件回滚

### 已经push
```shell
git log ${filename}
git checkout ${commit_id} ${filename}
git push origin ${branch}
```

## 如何在历史中找到引起错误的一次提交

使用 `git bisect`

```shell
git bisect start

git bisect good 最老的好的提交的哈希

git bisect bad 最新的坏的提交的哈希

# 当前的repo会被checkout 到中间的提交

# 如果当前的提交是坏的
git bisect bad
# 当前的repo会被checkout 到新的好的和坏的中间

# 如果当前的提交时好的
git bisect good

# 停止
git bisect reset

```

## 使用 access_token 来拉取源代码

```shell
git clone https://oauth2:token@github.ibm.com/user/repo
```

## 从仓库里下载文件夹

```bash
curl https://codeload.github.com/mui/material-ui/tar.gz/master | tar -xz --strip=2  material-ui-master/examples/nextjs
```

## 从仓库里下载指定文件

先在 GitHub 页面里找到 raw 文件的URL，私有仓库的话，URL后会自动带上token

```bash
curl -Lo README.md https://raw.githubusercontent.com/kubernetes-client/c/master/README.md
```

## git push

```bash
# 将本地 local-branch 的内容，上传到 remote 的 remote-branch 上
git push <remote> <local-branch>:<remote-branch>

# 如果 remote-branch 省略，则 remote-branch = local-branch
git push origin main //将本地的 main 分支上传到 origin 的 main 分支

git push upstream branch1 // 将本地的 branch1 分支 上传到 upstream 的 branch1 分支上。
```

## 让 Windows 平台上的 git bash 识别中文名

Step:

1. 创建文件

    ```bash
    touch ~/.bashrc
    ```

1. 文件内容如下：

    ```bash
    export LANG="zh_CN.UTF-8"
    export LC_ALL="zh_CN.UTF-8"
    ```
