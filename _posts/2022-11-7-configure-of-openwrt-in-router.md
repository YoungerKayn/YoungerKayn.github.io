---
layout: post
title: "记Reopen路由器配置过程"
subtitle: '充当备忘录'
author: "Younger"
header-style: text
header-img: "img/post-bg-configure-of-openwrt.jpg"
header-mask: 0.2
tags:
  - 笔记
  - openwrt
---
## 记Openwrt配置过程

**[本次刷写的固件](https://www.right.com.cn/forum/thread-837205-1-1.html)  **

1.   ### 上传脚本，解决校园网认证

2.   ### 配置opkg源

     大部分插件需通过opkg安装，因此需要先更新配置opkg源

     已知斐讯K1的处理器架构为32位的MT7620A，CPU Type为MIPS24KEc(mips_24kc)

     一开始因为错误使用了上一个固件使用的opkg源，导致更新opkg时架构出错，这一个固件应该使用[这一个软件源](https://downloads.openwrt.org/releases/packages-18.06/mipsel_24kc/)

     ```
     src/gz barrier_breaker_packages https://downloads.openwrt.org/releases/packages-18.06/mipsel_24kc/packages/
     src/gz barrier_breaker_base https://downloads.openwrt.org/releases/packages-18.06/mipsel_24kc/base/
     src/gz barrier_breaker_luci https://downloads.openwrt.org/releases/packages-18.06/mipsel_24kc/luci/
     src/gz barrier_breaker_routing https://downloads.openwrt.org/releases/packages-18.06/mipsel_24kc/routing/
     src/gz barrier_breaker_telephony https://downloads.openwrt.org/releases/packages-18.06/mipsel_24kc/telephony/
     ```

     配置完以上软件源即可更新opkg

     顺便可以安装SFTP所需的vsftpd、openssh-sftp-server，并通过以下命令开启SFTP服务器

     ```sh
     /etc/init.d/vsftpd start
     /etc/init.d/vsftpd enable
     ```

3.   ### 配置系统及无线网络

     修改root密码，添加lan口DNS

     上传登录脚本及ip变化检测脚本，并将登录脚本和ip变化检测加入计划任务

     修改无线网络的SSID、加密方式、密码、国家代码US

     通过DHCP分配IP

     进入网络-防火墙，允许wan口转发到lan口，以便通过校园网内网访问，然后配置端口转发，将服务映射到校园内网

     最后备份配置文件后即完成

     
