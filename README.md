# blogsrc
hexo init之后的源代码,用于生成blog

# 撰写新文章
在墙外的机器上，pull, hexo n "新文章题目"，commit and push

# 修改文章
在墙内的机器上，pull，修改source/_post/目录下的文章，修改完成后commit and push

# 发布
在墙外的机器上，pull, hexo clean,hexo g,hexo d

# 更换主题
在搜索引擎上搜索 "hexo 主题"

# 更换了运行blogsrc的机器，重新搭建环境

- 将blogsrc从github上clone下来
- 安装nodejs
- 安装hexo-cli
```
npm install -g hexo-cli --proxy http://127.0.0.1:7071
```
- 到blogsrc目录下
```
npm install hexo --save --proxy http://127.0.0.1:7071 
```
- 安装其他缺失的插件（npm ls --depth 0）
- 重新下载主题
- hexo clean,hexo g,hexo d


