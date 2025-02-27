# 从零开始配置NVIDIA Jetson Nano(踩坑史)

* [烧录](#烧录)
* [供电](#供电)
* [摄像机](#摄像机)
* [软件环境](#软件环境)
* [下载软件](#下载软件)
* [Resources](#resources)

------

## 烧录
1. 下载[Jetson 下载中心 | NVIDIA Developer](https://developer.nvidia.com/zh-cn/embedded/downloads)并解压得到镜像文件`sd-blob.img`
> ⚠️这里要注意自己板子的版本
> - 老板子需要安装4GB版本
> - 新板子说上一代性能过剩，需要安装2GB版本  *Jetson Nano 2GB Developer Kit*
> （在新板子里烧了两次4GB版本，一直没法点亮，……）
2. 烧录到microSD卡中
> 推荐使用官方推荐的[balenaEtcher - Flash OS images to SD cards & USB drives](https://www.balena.io/etcher/)
> 具体流程参考：[Jetson Nano 开发者套件入门 | NVIDIA Developer](https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit#write)
> 
3. 连接外设(鼠标、键盘、显示器)并启动

   ![](https://doublez-site-bed.oss-cn-shanghai.aliyuncs.com/img/20210112173123.png)

<br/>

## 供电

如果在开机过程中NVIDIA界面不停出现/消失/出现/消失，可能是由于机器启动到一半失败，又重新开机，这涉及到电源供电问题：
- 如果使用的是DP供电，需要安装跳线帽
- 要保证供电瓦数达到要求
- 线和头也要符合规范
[菜鸟手册（1）：给Jetson Nano安装DC电源](https://mp.weixin.qq.com/s?__biz=MjM5NTE3Nzk4MQ==&mid=2651234521&idx=1&sn=a2332ae5963fa8d150de53823e409149&chksm=bd0e744b8a79fd5dcc1888a1a764f10dfdddf1d5ffa3c7afa19dbb7ad949b010198aa9c8d91d&scene=21#wechat_redirect)

<br/>

## 摄像机

[菜鸟手册（2）：给Jetson Nano安装树莓派摄像头](https://mp.weixin.qq.com/s?__biz=MjM5NTE3Nzk4MQ==&mid=2651234579&idx=1&sn=7f10f030e9c60b15c6805fa1ea495347&chksm=bd0e75818a79fc977693c16d7eb4dd87709eab82542687208d5ba34cfcc877a2da68ab6a106b&scene=21#wechat_redirect)

```bash
gst-launch-1.0 nvarguscamerasrc ! 'video/x-raw(memory:NVMM),width=3820, height=2464, framerate=21/1, format=NV12' ! nvvidconv flip-method=0 ! 'video/x-raw,width=960, height=616' ! nvvidconv ! nvegltransform ! nveglglessink -e
```

一系列摄像头玩法：[GitHub - JetsonHacksNano/CSI-Camera: Simple example of using a CSI-Camera (like the Raspberry Pi Version 2 camera) with the NVIDIA Jetson Nano Developer Kit](https://github.com/JetsonHacksNano/CSI-Camera)

```bash
python simple_camera.py
python face_detection.py
```

![](https://doublez-site-bed.oss-cn-shanghai.aliyuncs.com/img/20210112173220.png)

<br/>

## 软件环境

- [切换python版本](https://github.com/doubleZ0108/Play-with-NVIDIA-Jetson-Nano/blob/master/linux/switch-py-version.md)
- python 环境：[jetson nano安装与配置_深度学习-CSDN博客](https://blog.csdn.net/l641208111/article/details/100152647)
> 注意里面说要改一段代码，不要改❌
- nvcc、opencv、cuDNN环境：[笔记（五）Jetson Nano 基础环境配置_SWorld-CSDN博客](https://blog.csdn.net/baidu_26678247/article/details/109009990)
- 关闭图形界面环境：Jetson Nano只有4G内存，图形界面会占用很大一部分内存(800M)
```bash
sudo init 3     # 关闭桌面
sudo init 5     # 重启桌面
```

<br/>

## 下载软件
⚠️注意Nano是arm64体系架构，安装软件时要选清楚版本
```bash
# 安装deb文件
sudo dpkg -i xxx.deb
```

<br/>

## Resources

- 官方资源下载：[Jetson Nano 开发者套件入门 | NVIDIA Developer](https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit#write)
-  [Nvidia Jetson Nano介绍与使用指南 - 知乎](https://zhuanlan.zhihu.com/p/319292104)
- [Jetson Nano从零开始（2）：硬件篇_Mingyong_Zhuang的技术博客-CSDN博客_jetson nano](https://blog.csdn.net/qqqzmy/article/details/96764071?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-2.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-2.control)