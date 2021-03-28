title: PyTorch笔记

date: 2020-09-03 20:58:28

categories:
- AI

tags:
- ai
- pytorch
---

## 介绍

PyTorch是Facebook开发的深度学习库，是谷歌Tensorflow的竞争对手，由于其API对新用户比较友好，所以在学术界非常流行。

下面介绍一下如何在Windows 10系统上安装PyTorch，以及如何使用。

本文只介绍pip安装PyTorch的方法。

## 安装

0. [PyTorch官方网站参考](https://pytorch.org/tutorials/beginner/blitz/tensor_tutorial.html#sphx-glr-beginner-blitz-tensor-tutorial-py)

1. 到Python官方网站下载Python3的安装包并在本地安装。

2. 使用pip安装numpy
```
pip3 install numpy
```

3. 使用pip安装PyTorch
```
pip3 install torch==1.2.0+cpu torchvision==0.4.0+cpu -f https://download.pytorch.org/whl/torch_stable.html
```

## 安装好之后的验证

```
python
from __future__ import print_function
import torch
x = torch.rand(5, 3)
print(x)
```

## 笔记

### 什么是“张量”

张量就是多维数组
