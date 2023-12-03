title: Python virtualenv requirements pytest jupyter

date: 2020-06-22 21:50:50

categories:

- Python

tags:

- python
- venv
- requirements
- pytest
- jupyter
- pyenv

---

## Python 虚拟环境

### 介绍

Python虚拟环境可以搭建一个当前工作的包依赖系统，所有的依赖包都下载到当前目录下，不会对系统的Python环境造成影响。

虚拟环境指的是多个依赖包环境共存，并不是多个python共存。所有的虚拟环境都使用一个python。

以前流行的是 `virtualenv`。从 Python 3.3 起，官方提供了一个相似的工具并推荐使用 `venv`

<!--more-->

### 安装 venv

不需要安装

### 在当前的项目目录下生成虚拟环境

```shell
python -m venv /path/to/new/virtual/environment
```

### 激活虚拟环境

```shell
source /path/to/new/virtual/environment/bin/activate
```

### 退出虚拟环境

```shell
/path/to/new/virtual/environment/bin/deactivate
```

## Python 包依赖

### 生成requirements.txt文件

```shell
pip freeze > requirements.txt
```

### 安装requirements.txt依赖

```shell
pip install -r requirements.txt
```

## pytest

### 安装

```shell
pip install pytest
py.test --version
```

### 编写test case

将文件命令为以 test_ 开头

```python
def func(x):
 return x+1

def test_func():
 assert func(3) == 4
```

### 运行

```shell
py.test
```

## jupyter notebook

### 安装 notebook

```shell
pip install notebook
```

### 运行 notebook

```shell
jupyter notebook
```

### 配置远程可访问

#### 生成配置文件

```shell
jupyter notebook --generate-config
```

#### 修改配置文件

```configuration
c.NotebookApp.ip='*' #×允许任何ip访问
```

## Matplotlib

python的绘图库，与Numpy一起使用，是MatLab的开源替代方案

## pyenv

安装和管理多个 Python 版本。
