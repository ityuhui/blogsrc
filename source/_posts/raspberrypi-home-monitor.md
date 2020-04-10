title: 树莓派实现家庭监控

date: 2014-10-18 23:22:08

categories:
- 树莓派

tags:
- 树莓派

---

## 器材

树莓派 B版本

摄像头  Z-Star Microelectronics Corp. ZC0301 Webcam

电源 5V2A

<!-- more -->

## 准备

摄像头需要调整，就是旋转摄像头前面的镜头对焦，否则拍出来的照片很模糊。

## fswebcam方案

### 安装

安装之前要升级一下树莓派系统

```shell
apt-get update
apt-get upgrade
```

安装fswebcam

```shell
apt-get install fswebcam
```

### 使用

发命令

```shell
fswebcam -r 640x480 -d /dev/video0 testpictire.jpg
```

就可以了，再写一个将这个文件发送到邮箱里的脚本，或者scp到自己的VPS上去，或者直接在树莓派上安装httpd或者samba

## motion 方案

### 安装

安装motion

```shell
apt-get install motion
```

### 配置

修改/etc/motion/motion.conf
修改下面三项：

这一项是因为我的摄像头只支持jpeg格式，你可能不需要修改

```
v4l2_palette 3  
```

为了能让外部机器访问

```
webcam_localhost off
```

自动保存的照片的地址：

```
target_dir 
```

然后启动

```shell
motion -n
```

### 使用

用浏览器打开ip:8081就可以观看视频了
