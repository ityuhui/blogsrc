title: Python yield 语法

date: 2023-10-18 20:55:30

categories:

- Python

tags:

- python

---

## yield

用于写生成器，但是可以用来写协程（现在用 async 库来写协程库更好）

```python
def f123():
    yield 1
    yield 2
    yield 3

for item in f123():
    print(item)
```

```bash
1
2
3
```
