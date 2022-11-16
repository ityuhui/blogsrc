title: Python virtualenv requirements pytest jupyter
date: 2020-06-22 21:50:50

categories:
- Python

tags:
- python
- virtualenv
- requirements
- pytest
- jupyter

---

## Python 虚拟环境

### 介绍

Python虚拟环境可以搭建一个当前工作的包依赖系统，所有的依赖包都下载到当前目录下，不会对系统的Python环境造成影响。

虚拟环境指的是多个依赖包环境共存，并不是多个python共存。所有的虚拟环境都使用一个python。

下面介绍的是 `virtualenv`, 但是从 Python 3.3 起，官方也提供并推荐使用 `venv`， 本文未来会进行修改，介绍 `venv`

<!--more-->

### 安装 virtualenv

```shell
virtualenv --version #查看是否已经安装
pip install virtualenv
```

### 在当前的项目目录下生成虚拟环境

```shell
virtualenv ${virtual_env_name}
```

### 激活虚拟环境

```shell
${virtual_env_name}/script/activate
```

### 退出虚拟环境

```shell
${virtual_env_name}/script/deactivate
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

### 安装

```shell
pip install notebook
```

### 运行

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