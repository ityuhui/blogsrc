# blogsrc

hexo init之后的源代码,用于生成blog

## 日常使用

### 撰写新文章

```shell
git pull
hexo n "新文章题目"
git commit
git push
```

### 修改文章

```shell
git pull
修改source/_post/目录下的文章,修改完成后
git commit
git push
```

### 本地测试

```shell
hexo server
```

### 发布

```shell
git pull
hexo clean
hexo g
hexo d
```

### 更换主题

在搜索引擎上搜索 "hexo 主题"

## 更换了运行blogsrc的机器，重新搭建环境

### 将blogsrc从github上clone下来

### 安装nodejs

### 安装hexo-cli

```shell
npm install -g hexo-cli --proxy http://127.0.0.1:7071
```

### 到blogsrc目录下

```shell
npm install hexo --save --proxy http://127.0.0.1:7071 
```

### 安装其他缺失的插件

先使用（npm ls --depth 0）查找缺失的包

再使用npm install来安装

### 重新下载主题

### 正常使用

hexo clean,hexo g,hexo d

## 设置或取消 github pages 定制域名

- blogsrc 源代码文件：

  https://github.com/ityuhui/blogsrc/blob/master/_config.yml

- ityuhui.github.io 在 github 上的配置页面：

  https://github.com/ityuhui/ityuhui.github.io/settings/pages

## 支持本地搜索

1.安装插件

```bash
npm install hexo-generator-searchdb
```

2.更新配置文件，新增：

```yaml
search:
  path: search.xml
  field: post
  content: true
  format: html
```

3.更新主题的配置文件，修改：

```yaml
local_search:
  enable: true
```
