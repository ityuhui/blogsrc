title: Ubuntu 使用总结

date: 2020-12-31 11:23:35

categories:
- Linux

tags:
- linux
- ubuntu

---

## 升级GCC

```shell
sudo apt update
sudo apt install gcc-8
sudo apt install g++-8
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-8 800 --slave /usr/bin/g++ g++ /usr/bin/g++-8
```
