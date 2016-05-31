---
layout: post
title: 运行第一个 Cuda 程序
category: 技术
---
`Cuda` 中的 Hello World.

Platform: `SUSE Linux`

CUDA Version: `cuda_7.5.18_linux`

# 安装软件

安装驱动和 `Cuda`, 安装的时候发现 `Cuda` 中包含驱动了, 所以只下载 [Cuda][1] 即可, 不需要单独下载驱动.

# 环境变量

确保:

- `/usr/local/cuda-7.5` 在 `PATH` 中
- `/usr/local/cuda-7.5/lib64` 在 `LD_LIBRARY_PATH` 中, 或在 `/etc/ld.so.conf` 中加入 `/usr/local/cuda-7.5/lib64` 并运行 `ldconfig` 使之生效.

使用 `nvcc` 命令测试是否设置成功.

## 设置和查看环境变量

```shell
export PATH="<PATH DIR>:$PATH"
# 在 /etc/profile 中添加变量

source /etc/profile
# 使设置生效

export
# 查看PATH

echo $PATH
# 单独查看 PATH
```


# 运行
```shell
nvcc helloCuda.cu
# 编译 helloCuda.cu
./a.out`
# 运行
```

[1]:https://developer.nvidia.com/cuda-downloads "CUDA 7.5 Downloads"
[2]:http://blog.csdn.net/dlutbrucezhang/article/details/8811456 "linux下查看和添加PATH环境变量"
