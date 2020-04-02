# blogsrc
hexo init之后的源代码,用于生成blog

# 撰写新文章

```shell
git pull
hexo n "新文章题目"
git commit
git push
```

# 修改文章

```shell
git pull
修改source/_post/目录下的文章,修改完成后
git commit
git push
```

# 本地测试

```shell
hexo server
```

# 发布

```shell
git pull
hexo clean
hexo g
hexo d
```

# 更换主题

在搜索引擎上搜索 "hexo 主题"

# 更换了运行blogsrc的机器，重新搭建环境

## 将blogsrc从github上clone下来

## 安装nodejs

## 安装hexo-cli

```shell
npm install -g hexo-cli --proxy http://127.0.0.1:7071
```

## 到blogsrc目录下

```shell
npm install hexo --save --proxy http://127.0.0.1:7071 
```

## 安装其他缺失的插件

### 先使用（npm ls --depth 0）查找缺失的包

### 再使用npm install来安装

## 重新下载主题

## hexo clean,hexo g,hexo d


