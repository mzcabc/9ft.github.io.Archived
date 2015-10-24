---
layout: post
title: 运行第一个Cuda程序
category: 技术
tags:
keywords:
description:
---
Cuba 中的 Hello World

Platform: SUSE Linux

CUDA Version: cuda_7.5.18_linux.run

# 安装软件
安装驱动和CUDA，安装驱动的时候发现CUDA中包含驱动了，所以只[下载][1]安装CUDA即可。

# 环境变量
确保:
>- `/usr/local/cuda-7.5` 在`PATH`中
>- `/usr/local/cuda-7.5/lib64` 在 `LD_LIBRARY_PATH` 中, 或在`/etc/ld.so.conf`中加入`/usr/local/cuda-7.5/lib64`并运行`ldconfig`使之生效。
使用`nvcc`命令测试是否成功。

## 设置和查看环境变量
在`/etc/profile`中添加`export PATH="<PATH DIR>:$PATH"`,

`source /etc/profile`使之生效

`export`: 查看PATH
`echo $PATH`: 单独查看 PATH

# 运行
`nvcc helloCuda.cu` :编译 helloCuda.cu
`./a.out` : 运行

[1]:https://developer.nvidia.com/cuda-downloads
[2]:http://blog.csdn.net/dlutbrucezhang/article/details/8811456 "linux下查看和添加PATH环境变量"
