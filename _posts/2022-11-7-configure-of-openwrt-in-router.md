---
layout: post
title: "记Openwrt路由器配置过程"
subtitle: '充当备忘录'
author: "Younger"
header-style: text
header-img: "img/post-bg-test-post.png"
header-mask: 0.2
tags:
  - 笔记
  - openwrt
---
## 记Openwrt配置过程

**[本次刷写的固件](https://www.right.com.cn/forum/thread-837205-1-1.html)  **

1.   ### 通过ipk安装curl，解决校园网认证

     curl需通过opkg安装，因此需要先更新配置opkg源

     已知斐讯K1的处理器架构为32位的MT7620A，CPU Type为MIPS24KEc(mips_24kc)
     
     一开始因为错误使用了上一个固件使用的opkg源，导致更新opkg时架构出错，这一个固件应该使用[这一个软件源](https://downloads.openwrt.org/releases/packages-18.06/mipsel_24kc/)
     
     配置完opkg源就可以安装curl并通过我的登录脚本通过校园网认证了
     
     顺便可以安装SFTP所需的vsftpd、openssh-sftp-server，并通过以下命令开启SFTP服务器
     
     ```sh
     /etc/init.d/vsftpd start
     /etc/init.d/vsftpd enable
     ```
     
1.   ### 配置系统及无线网络

     修改root密码，添加lan口DNS

     上传登录脚本及ip变化检测脚本，并将登录脚本和ip变化检测加入计划任务
     
     修改无线网络的SSID、加密方式、密码、国家代码US
     
     通过DHCP分配IP
     
     关闭lan口、wan口防火墙，以便通过校园网内网访问
     
     最后备份配置文件后即完成
     
     
