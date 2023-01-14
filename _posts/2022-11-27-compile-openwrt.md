---
layout: post

title: "编译一个Openwrt固件"

subtitle: "Compile openwrt"

author: "Younger"

header-style: text

tags:
  - 笔记
  - openwrt
---

## Openwrt 固件编译

- 基于 Ubuntu22.04.1 桌面版（Vmware）

     1.   ### 安装编译环境依赖

           先更新apt软件列表

           ``` shell
           sudo apt full-upgrade -y && sudo apt update -y
           ```

           通过apt安装依赖

           ``` shell
           sudo apt install -y ack antlr3 aria2 asciidoc autoconf automake autopoint binutils bison build-essential bzip2 ccache \
           cmake cpio curl device-tree-compiler fastjar flex g++-multilib gawk gcc-multilib gettext git git-core gperf haveged \
           help2man intltool lib32gcc-s1 libc6-dev-i386 libelf-dev libglib2.0-dev libgmp3-dev libltdl-dev libmpc-dev libmpfr-dev \
           libncurses5-dev libncursesw5-dev libpython3-dev libreadline-dev libssl-dev libtool libz-dev lrzsz mkisofs msmtp nano \
           ninja-build p7zip p7zip-full patch pkgconf python2.7 python3 python3-pip qemu-utils rsync scons squashfs-tools \
           subversion swig texinfo uglifyjs unzip upx upx-ucl vim wget xmlto xxd zlib1g-dev
          ```

     2.   ### 首次编译

           **以下操作需在非root用户进行**
           **以下操作需在非root用户进行**
           **以下操作需在非root用户进行**

           设置系统代理并为git设置代理

           ``` shell
           git config --global http.proxy socks5://192.168.7.3:7890
           git config --global https.proxy socks5://192.168.7.3:7890
           ```

           通过[这个网址](https://www.ipaddress.com/site/github.com)获取github的IP并修改/etc/hosts如下

           ``` shell
           IP github.com
           IP www.github.com
           ```

           拉取源码

           ``` shell
           git clone https://github.com/coolsnowwolf/lede.git openwrt
           ```

           添加自定义源

           ``` shell
           cd ~/openwrt
           cat >> feeds.conf.default <<EOF
           src-git kenzo https://github.com/kenzok8/openwrt-packages
           src-git passwall https://github.com/xiaorouji/openwrt-passwall
           EOF
           ```

           更新并拉取

           ``` shell
           ./scripts/feeds update -a
           ./scripts/feeds install -a
           ```

           单独添加软件包（可选）

           *例：添加fcgiwrap用于包装luci*

           ``` shell
           git clone https://github.com/yhfudev/openwrt-fcgiwrap.git package/feeds/packages/fcgiwrap
           ```

           更改主题（可选）

           ``` shell
           # 删除自定义源默认的 argon 主题
           rm -rf package/lean/luci-theme-argon

           # 部分第三方源自带 argon 主题，上面命令删除不掉的请运行下面命令
           find ./ -name luci-theme-argon | xargs rm -rf;

           # 针对 LEDE 项目拉取 argon 原作者的源码
           git clone -b 18.06 https://github.com/jerrykuku/luci-theme-argon.git package/lean/luci-theme-argon

           # 替换默认主题为 luci-theme-argon
           sed -i 's/luci-theme-bootstrap/luci-theme-argon/' feeds/luci/collections/luci/Makefile
           ```

           设置路由器默认的LAN IP（可选）

           ``` shell
           # 设置默认IP为 192.168.7.1
           sed -i 's/192.168.1.1/192.168.7.1/g' package/base-files/files/bin/config_generate
           ```

           *   #### 两种编译方式：

               *   ##### 云端编译（仅制作.config文件）

               1.   ###### 配置编译选项

                    ``` shell
                    make menuconfig
                    ```

               2.   ###### 通过以下命令获得 `seed.config` 配置文件

                    ```shell
                    # 若在调整OpenWrt系统组件的过程有多次保存操作，则建议先删除.config.old文件再继续操作
                    rm -f .config.old

                    # 根据编译环境生成默认配置
                    make defconfig

                    # 对比默认配置的差异部分生成配置文件（可以理解为增量）
                    ./scripts/diffconfig.sh > seed.config
                    ```

               3.   ###### 使用 GitHub Ac­tions 云编译

               *   ##### 本地编译

               1.   ###### 配置编译选项

                    ``` shell
                    make menuconfig
                    ```

               2.   ###### 下载编译所需的软件包

                    ``` shell
                    make download -j8 V=s
                    ```

               3.   ###### 编译

                    ```shell
                    make -j1 V=s
                    ```

                    *若第一次编译，建议使用单线程编译，可提高编译成功率，但过程非常漫长，取决于机器的性能*

                    编译完成后固件输出路径： `/openwrt/bin/targets/`

     3.   ### 二次编译

           1.   #### 更新本地编译环境

                ```shell
                # 更新软件列表、升级软件包
                sudo sh -c "apt update && apt upgrade -y"

                # 拉取最新源码
                cd ~/openwrt && git pull

                # 更新下载安装订阅源包含的软件包
                cd ~/openwrt
                ./scripts/feeds update -a && ./scripts/feeds install -a
                ```

           2.   #### 清理旧文件

                ```shell
                # 删除/bin和/build_dir目录中的文件
                make clean
                ```

                如果要更换架构，建议执行以下命令深度清理 `/bin` 和 `/build_dir` 目录的中的文件 (`make clean`) 以及 `/staging_dir`、`/toolchain`、`/tmp` 和 `/logs` 中的文件

                ```shell
                make dirclean
                ```

                如果需要对组件重新调整，则先删除旧配置

                ```shell
                rm -rf ./tmp && rm -rf .config
                ```

                再次配置编译选项

                ```shell
                make menuconfig
                make download -j8 V=s
                ```

           3.   #### 编译

                ```shell
                make -j$(nproc) || make -j1 || make -j1 V=s
                ```

                *二次编译可以优先使用多线程，报错会自动使用单线程，仍然报错会单线程执行编译并输出详细日志*

     4.  ### 利用x86架构在Vmware中进行测试
           *   路由刷机有风险，编译后可先使用x86版本的固件使用vmware测试，平台选择x86，目标image选择vmdk。vmware新建虚拟机使用已存在的磁盘，硬盘一定要选择IDE格式，不要选择SCSI

            1.  #### 安装qemu-img
                ```shell
                sudo apt-get install qemu-utils
                ```

            2.  #### 转换
                ```shell
                qemu-img convert -f raw openwrt-x86-generic-combined-ext4.img -O vmdk openwrt-x86-generic-combined-ext4.vmdk
                ```

            3.  #### 创建虚拟机
                将上面转换后的.vmdk文件传输到主机上，在Vmware新建虚拟机
                按如下步骤操作：
                自定义 - 稍后安装操作系统 - Linux - 版本 Ubuntu - 选择虚拟机位置 - 处理器核心数 1 - 内存 1024M - 网络类型 NAT - I/O控制器类型 LSI Logic - 磁盘类型 IDE - 使用现有虚拟磁盘 - 选择转换好的.vmdk - 安装并启动

            4.  #### 修改内网配置

                查看IP地址

                ```shell
                ifconfig br-lan
                ```

                查看虚拟机NAT模式的子网地址

                修改 /etc/config/network 配置br-lan的IP地址，使之处于NAT模式所在的子网

                重启虚拟机后主机应可以ping通虚拟机

            5.  #### 配置外网访问

                在主机中访问web管理界面，默认密码: password

                选择Network -> Interfaces，点击br-lan后面的Edit按钮

                将协议修改为DHCP client，并点击Switch protocol按钮，否则修改不会生效

                此时ip地址已经发生了改变，所以web连接断掉了，在虚拟机中查看br-lan地址，
                并ping外网，已经可以ping通了

  > 参考：[OpenWrt 固件编译教程](https://blog.csdn.net/qq84395064/article/details/127934147)
