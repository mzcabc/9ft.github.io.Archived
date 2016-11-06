

Mac 原生读写挂载 NTFS

```shell
$ sudo -s	# 切换 root 用户
$ cd /sbin	# 进入 /sbin 文件夹
$ mv mount_ntfs mount_ntfs_orig	# 重命名
$ vi mount_ntfs	# 新建 mount_ntfs 文件
```

写入:

```shell
#!/bin/sh
/sbin/mount_ntfs_orig -o rw,nobrowse "$@"
```

```shell
$ chmod a+x mount_ntfs	# 增加可执行权限
$ exit	# 退出 root 身份
```

重启验证

如果遇到问题: mv: rename mount_ntfs to mount_ntfs_orig: Operation not permitted.

操作如下:

1) 重启, `cmd+R` 进入恢复 (recovery) 模式
2) terminal 下输入: `$ csrutil disable`
4) `$ reboot` 重启



Source:

https://www.zhihu.com/question/19571334/answer/89658747

