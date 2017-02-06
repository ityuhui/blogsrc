# blogsrc
hexo init之后的源代码,用于生成blog

# 撰写新文章
在墙外的机器上，pull, hexo n "新文章题目"，commit and push

# 修改文章
在墙内的机器上，pull，修改source/_post/目录下的文章，修改完成后commit and push

# 发布
在墙外的机器上，pull, hexo clean,hexo g,hexo d

# 更新主题
```
cd themes/landscape-plus
git pull
```
