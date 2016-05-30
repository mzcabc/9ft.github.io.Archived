---
layout: post
title: VS 错误: 本地调试时出现远程调试器错误
date: 2016-05-30 21:27:20 +0800
category: 技术
---
最近在用 `VS2013` 跑程序, 经常出现一个错误. 而且看起来是个无厘头的错误.

//TODO 图待补充

错误内容:  
`无法启动程序 远程调试器拒绝了连接请求. 请确保远程调试器在"Windows 身份验证"模式下运行.`

用中文查了一下资料未果.

冷的一看, 我没有远程调试程序啊, 我的程序是本地调试运行的. 调试应该走的网络协议. 不过我把 Win 自带的杀软和防火墙关掉还是未果.

用英文才查到 MSDN 的一篇文档 [`Unable to Connect to the Microsoft Visual Studio Remote Debugging Monitor`][Unable to Connect to the Microsoft Visual Studio Remote Debugging Monitor]

>**I got this message while I was debugging locally**

>If you are getting this message while you are debugging locally, your anti-virus software or a third-party firewall may be to blame. Visual Studio is a 32-bit application, so it uses the 64-bit version of the remote debugger to debug 64-bit applications. The two processes communicate using the local network within the local computer. No network traffic leaves the computer, but it is possible that third party security software may block the communication.

跟自己预想的差不多, 实际上调试走的是网络, 罪魁祸首是 *杀毒软件* 或者 *防火墙* 的问题. **管理员运行 `devenv.exe` 成功解决**.

附 `VS` 本地调试需要的网络条件: [`Error: Firewall on Local Machine`][Error: Firewall on Local Machine]

# 后记

对比一下 MSDN 的中英文档就会发现, 中文的说明太少了, 然而中文资料基本没有, 所以光查中文没法解决这个问题.

英文页面: https://msdn.microsoft.com/en-us/library/0773txhx.aspx
中文页面: https://msdn.microsoft.com/zh-cn/library/0773txhx.aspx

[Unable to Connect to the Microsoft Visual Studio Remote Debugging Monitor]:https://msdn.microsoft.com/en-us/library/0773txhx.aspx

[Error: Firewall on Local Machine]:https://msdn.microsoft.com/en-us/library/2y0675c1.aspx
