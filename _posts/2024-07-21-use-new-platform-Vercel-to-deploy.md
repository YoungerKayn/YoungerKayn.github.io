---
layout: post

title: "使用Vercel进行blog部署"

subtitle: "放弃Github Pages"

author: "Younger"

header-style: text

tags:
  - 笔记
  - blog
  - Vercel
  - Github Pages
  - cloudflare
  - deploy
---

### Github Pages 在中国大陆的访问速度较慢，并且在博客更新后，部署速度很慢，对于静态资源的更新不利。
### Vercel 拥有更好的访问体验已经更快的部署速度，因此启用Pages而使用Vercel。

 使用Vercel部署huxpro.github.io的步骤很简单，注册账号后，通过Github导入对应的项目，直接部署即可。

**Openwrt building config maker / Openwrt编译配置生成工具**

### Intro / 说明
以下是为检测Vercel部署速度而写的内容。

飞光飞光，劝尔一杯酒。吾不识青天高，黄地厚，唯见月寒日暖，来煎人寿。食熊则肥，食蛙则瘦。神君何在，太一安有？
天东有若木，下置衔烛龙。吾将斩龙足，嚼龙肉，使之朝不得回，夜不得伏。自然老者不死、少者不哭。何为服黄金，吞白玉？
谁似任公子，云中骑碧驴。刘彻茂陵多滞骨，嬴政梓棺费鲍鱼。

兵火有余烬，贫村才数家。无人争晓渡，残月下寒沙。

秋风清，秋月明。落叶聚还散，寒鸦栖复惊。相思相见知何日，此时此夜难为情。

## Usage / 用法
  
  
>**English**
## Openwrt building config maker

 Make your own Openwrt build config file by the GitHub Action, save your time from setting up the build environment.

### Usage

- Click [HERE](https://github.com/YoungerKayn/Config-Maker/generate) to create a new repository

- Select `Make Config File` on the Actions page

- Click the `Run workflow` button

- Refresh the page and click the lastest `Make Config File` action to access its log

- Click on the `build` job on the left sidebar

- Wait for the `SSH connection to Actions` job to be executed at this page, and information like the following will appear:

    ``` shell
    To connect to this session copy and paste the following into a terminal or browser:
    CLI: ssh McuGhq5PvktJyr53WHFsyjLMW@nyc1.tmate.io
    URL: https://tmate.io/t/McuGhq5PvktJyr53WHFsyjLMW
    TIPS: Run 'touch /tmp/continue' to continue to the next step.
    ```

    then follow the tips to SSH to Actions. (If you are using a web terminal and get a black screen, just press `Ctrl+C`)

- Type `cd openwrt && make menuconfig` in the terminal to start building your .config file

- After you finishing your work, type `touch /tmp/continue` in the terminal then exit the terminal, the rest of the work will be done automatically

- ~~Star this repo, then it will be done faster~~

- After all the work is done, you will see `OpenWrt_config` in `Artifacts` on the Action `Summary` page, just click and download it

### Advanced Usage

1. **custom source code**  
    This repo will use [LEDE](https://github.com/coolsnowwolf/lede)'s source code to build the environment by default, you can change it in file `.github/workflows/make-config.yml` 

2. **custom fireware envs**  
    You can change your firmware's default IP, hostname, theme, add/remove packages and so on by adding command in `./github/diy-part1.sh` and `./github/diy-part2.sh`

3. **custom feeds source**  
    You can add your own feeds by modifying `./github/feeds.conf.default`. For example, add `src-git luci https://github.com/coolsnowwolf/luci` to use the packages of [LuCI](https://github.com/coolsnowwolf/luci)
  
  
>**中文**
## Openwrt编译配置生成工具
 通过Github Actions来构建你的Openwrt编译配置文件(.config)，免受自行搭建编译环境的痛苦，并可以配合 [P3TERX/Actions-OpenWrt](https://github.com/P3TERX/Actions-OpenWrt) 等在线编译项目，在完全不搭建编译环境的情况下编译自己的固件。
 >实际上，此项目就是从 [P3TERX/Actions-OpenWrt](https://github.com/P3TERX/Actions-OpenWrt) 分离出的的模块的改良版

### 用法
- 点击[这里](https://github.com/YoungerKayn/Config-Maker/generate)生成一个新的仓库
- 在Actions页面中，选择 `Make Config File`
- 点击 `Run workflows`
- 刷新页面后，你将会看到Actions执行了新的工作，点击它以进入日志页面
- 点击左侧边栏中的 `build` 
- 在这个页面上等待Actions执行到 `SSH connection to Actions` ,然后你会看到类似下面的信息：
    ``` shell
    To connect to this session copy and paste the following into a terminal or browser:
    CLI: ssh McuGhq5PvktJyr53WHFsyjLMW@nyc1.tmate.io
    URL: https://tmate.io/t/McuGhq5PvktJyr53WHFsyjLMW
    TIPS: Run 'touch /tmp/continue' to continue to the next step.
    ```
    点击URL链接或者在命令行中输入上面的ssh命令，就可以与Actions建立SSH连接（如果你进入网页终端时黑屏，按下Ctrl+C即可）
- 在终端中输入 `cd openwrt && make menuconfig` ，然后就可以开始构建你的配置文件了
- 完成并**保存**你的配置后，在终端中输入 `touch /tmp/continue` 命令就可以关闭终端了，剩下的工作会自动完成
- ~~给这个项目点一个 star 会让工作进行得更快~~
- 所有工作结束后，点击左侧的 `Summary` ，可以看到一个 `Openwrt_config` 文件，点击就可以下载你的 .config 文件

### 进阶用法
1. **自定义源码**  
  本仓库默认使用[LEDE](https://github.com/coolsnowwolf/lede)的源码进行环境搭建，你可以通过修改 `./github/workflows/make-config.yml` 中的相关变量来修改代码源
2. **自定义固件**  
  你可以通过在 `./github/diy-part1.sh` 或 `./github/diy-part2.sh` 中添加自定义命令来让Actions在 `make menuconfig` 前或后执行，从而修改固件的默认 IP、主机名、主题、添加 / 删除软件包等
3. **自定义软件源**  
  如果你需要添加自定义的软件源，你可以修改`./github/feeds.conf.default`。例如，在该文件中添加 `src-git luci https://github.com/coolsnowwolf/luci` 以使用[LuCI](https://github.com/coolsnowwolf/luci)中的软件包

## Credit / 鸣谢

- [Microsoft Azure](https://azure.microsoft.com)
- [GitHub Actions](https://github.com/features/actions)
- [OpenWrt](https://github.com/openwrt/openwrt)
- [Lean's OpenWrt](https://github.com/coolsnowwolf/lede)
- [tmate](https://github.com/tmate-io/tmate)
- [mxschmitt/action-tmate](https://github.com/mxschmitt/action-tmate)
- [csexton/debugger-action](https://github.com/csexton/debugger-action)
- [ActionsRML/delete-workflow-runs](https://github.com/ActionsRML/delete-workflow-runs)
- [Actions-OpenWrt](https://github.com/P3TERX/Actions-OpenWrt)