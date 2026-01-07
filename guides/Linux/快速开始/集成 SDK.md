<!--和互动直播对应文档基本相同，替换产品名称后即可重用-->

本文为您介绍 Linux 端集成 SDK 的操作步骤，帮助您快速集成 SDK，可以使用音视频通话的基本功能。

## <span id="前提条件">前提条件</span>

在开始运行工程之前，请您准备以下开发环境：

- 通用开发环境
    - 操作系统：Ubuntu 18.04+ 或 Centos 对等版本
    - CPU 架构：x86_64
    - 编译器：g++ (GNU)
    - 依赖：GLIBC 2.19+
    - 音频依赖：PulseAudio
::: note notice  
* PuseAudio是 Linux 桌面系统使用的音频服务子系统，对麦克风和扬声器的操作均依赖于它
* 若未安装或启动PulseAudio服务和安装客户端库（libpulse.so），音频依赖会降级到 Dummy Audio, 后续只能通过自定义音频输入和渲染进行音频推拉流。
:::
   
- 国产化开发环境
    - 操作系统：UOS 20+
    - CPU 架构：arm64 (aarch64, armv8)
    - 编译器：g++ (GNU)
    - 依赖：GLIBC 2.23+
    - 音频依赖：PulseAudio
    - 国产化：已取得飞腾 FT-2000, 腾锐 D2000, 鲲鹏 920, 麒麟 9006C, 麒麟 990，麒麟 CVE300 等处理器统信软件认证

![证书.png](https://yx-web-nosdn.netease.im/common/e01c12489fb511d51a076946f7f8ae25/证书.png)


## <span id="SDK目录">SDK 目录</span>


目录| 文件/文件夹名称 |是否必选| 说明 |
| ---| --- | --- |---|
lib| libnertc_sdk.so |是| 音视频通话基础模块。|
^^| libprotoopp.so |<ul><li>5.5.21 之前：是<li> V5.5.21 及之后版本：无| 网络通信模块。|
^^| libNERtcAiDenoise.so | 否 | AI 降噪插件（自 V4.6.40 起提供。）|

include| 以实际目录中的头文件为准 | 否 | API 头文件，导入后可以方便查看 API 注释|

<note type="note">
目前线上仅提供 x86_64 和 arm64 架构的 SDK, 如需要 arm (armeabi, armeabihf) 架构的 SDK，请联系技术支持或在线客服提供。
</note>

## <span id="集成 NERtc SDK">集成 NERtc SDK</span>

1. 在[云信 SDK 下载中心](https://yunxin.163.com/im-sdk-demo?category=nrtc)获取当前最新版本的 NERTC SDK，或者联系网易云信技术支持获取对应版本的 SDK。

2. 将 **include** 文件夹添加到工程项目的 **INCLUDE** 目录下, 并确认追加编译选项`-I`。

3. 将 **lib** 文件夹添加到工程项目的 **LIB** 目录下，并确认追加链接选项`-L`和`-l`。

4. 执行编译。

5. 运行应用前请确保可以链接到SDK 动态库，或使用`export LD_LIBRARY_PATH=Your/Sdk/Lib/Path`导入动态库搜索路径。

6. SDK 插件库（比如Ai 降噪）请确保放置于应用进程目录。

## <span id="后续步骤">后续步骤</span>

[实现音视频通话](https://doc.yunxin.163.com/nertc/quick-start/TYxNDEwNTA?platformId=50136)
