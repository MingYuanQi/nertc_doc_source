<!--keywords: 实时音视频,音视频通话,webrtc,集成RTC,集成 SDK -->

<!--和互动直播对应文档基本相同，替换产品名称后即可重用-->

本文为您介绍 Windows 端集成 SDK 的操作步骤，帮助您快速集成 SDK，可以使用音视频通话的基本功能。

## <span id="前提条件">前提条件</span>

在开始运行工程之前，请您准备以下开发环境：

- 开发环境：Microsoft Visual Studio 2017（推荐）或以上版本
- 操作系统：Microsoft Windows 7 或以上版本
- 编译器：Microsoft Visual C++ 2017 或以上版本

## <span id="SDK目录">SDK 目录</span>


目录| 文件/文件夹名称 |是否必选| 说明 |
| ---| --- | --- |---|
dll| nertc_sdk.dll |是| 音视频通话基础模块。                                     |
^^| protoopp.dll |<ul><li>5.5.21 之前：是<li> V5.5.21 及之后版本：无| 网络通信模块。<note type=notice> 自 5.5.21 版本起，该文件移除，相关功能整合到 nertc_sdk.dll。</note>| 
^^| NERtcnn.dll | <ul><li>V4.6.20 ～V4.6.X：是<li> V5.3.0 及之后版本：否 |机器学习模块（自 V4.6.20 起提供）。<note type="notice"> <ul><li>V4.6.20 ～V4.6.X 版本：该动态库必选。<li> V5.3.0 及之后版本，只有集成美颜、虚拟背景、音频啸叫检测、音频 AI 降噪、人脸增强、屏幕增强插件时，才需要集成该库文件。基础音视频不需要集成该库文件。</note>|
^^| libfreetype-6.dll |否（纯音频包非必选）| 视频相关模块。        |
^^| libjpeg-9.dll | ^^|^^             |
^^| libpng16-16.dll| ^^| ^^|
^^| libtiff-5.dll|^^ |^^ |
^^| libwebp-7.dll|^^ |^^ |
^^| SDL2.dll| ^^|^^ |
^^| SDL2_image.dll |^^ | ^^ |
^^| SDL2_ttf.dll | ^^ |^^|
^^| video_render.fxo |^^ |^^|
^^| NERtcBeauty.dll| 否 | 美颜插件（自 V4.6.20 起提供）。|
^^| NERtcFaceDetect.dll | 否 |人脸检测插件（自 V4.6.20 起提供）。|
^^| NERtcPersonSegment.dll | 否 |虚拟背景插件（自 V4.6.20 起提供）。|
^^| NERtcAiDenoise.dll | 否 | AI 降噪插件（自 V4.6.40 起提供。）|
^^| NERtcAiHowling.dll | 否 | AI 啸叫检测插件（自 V4.6.40 起提供。）|
^^| NERtcFaceEnhance.dll | 否 | 人脸增强插件（自 V5.3.0 起提供。）|
^^| NERtcVideoDenoise.dll | 否 | 视频降噪插件（自 V5.3.0 起提供。）|
^^| NERtcSuperResolution.dll | 否 | 视频 AI 超分插件（自 V5.3.0 起提供。）|
^^| NERtcScreenShareEnhance.dll | 否 | 屏幕增强插件（自 V5.3.0 起提供。）|
^^ |NERtcAudio3D.dll| 否 | 空间音效插件（自 V5.4.0 起提供。）|
lib| nertc_sdk.lib |否（使用动态加载的方式运行程序的情况下只需要 dll） | 包含了函数所在的 DLL 文件和文件中函数位置的信息。 |
api| 以实际目录中的头文件为准 | 否 | API 头文件，导入后可以方便查看 API 注释|

## <span id="集成 NERtc SDK">集成 NERtc SDK</span>

### 步骤1 （可选）新建项目


::: details 介绍如何新建项目，如果集成到已有的项目，请忽略该步骤。
1. 打开 Microsoft Visual Studio，单击**创建新项目** ，新建一个类型为 **MFC 应用**的项目。

    ![新建MFC应用.png](https://yx-web-nosdn.netease.im/common/924b9d6846e69b51d6aa9f90b53778d5/新建MFC应用.png)


2. 在 **MFC 应用程序**页面，选择**应用程序类型**为**基于对话框**，单击**完成**。

    ![MFC应用程序类型.png](https://yx-web-nosdn.netease.im/common/2899d7edf7b775874aacd58743f098fe/MFC应用程序类型.png)

    ::: note note
    不同版本的 Microsoft Visual Studio，界面存在差异，本文以Visual Studio 2022版本为例，其他版本的操作请以实际界面为准。
    :::

  
:::
### 步骤2 导入 SDK
1. 在<a href="https://doc.yunxin.163.com/nertc/sdk-download" target="_blank">云信 SDK 下载中心</a>获取当前最新版本的 NERTC SDK。

    若要使用其他版本，请联系网易云信技术支持获取对应的版本号。

2. 将解压后的 NERTC SDK 文件夹（本文以 nertc_sdk 为例）拷贝至 **NERTC.vcxproj** 所在目录，路径类似如下图所示。

    
    ![拷贝文件夹_windows.png](https://yx-web-nosdn.netease.im/common/27395c5626cec993140d99f288e1d68f/拷贝文件夹_windows.png)

    根据您需要集成的能力，选择对应的文件加入到工程路径下，具体如下表所示。

    功能/插件 | 动态库 
    ---- | -------------- | ---------
    所有能力|所有 `dll` 动态库
    音频 | <ul> <li>`nertc_sdk.dll`<li>`protoopp.dll`<li>`NERtcnn.dll`：<ul><li>V4.6.20 ～V4.6.X 版本：该动态库必选。<li> V5.3.0 及之后版本，只有集成音频啸叫检测插件时，才需要集成该库文件。基础音视频不需要集成该库文件。
    音视频 |<ul> <li>`nertc_sdk.dll`<li>`protoopp.dll`<li>`NERtcnn.dll`：<ul><li>V4.6.20 ～V4.6.X 版本：该动态库必选。<li> V5.3.0 及之后版本，只有集成美颜、虚拟背景、音频啸叫检测、音频 AI 降噪、人脸增强、屏幕增强插件时，才需要集成该库文件。基础音视频不需要集成该库文件。</ul><li> 视频相关模块：`libfreetype-6.dll`、`libjpeg-9.dll`、`libpng16-16.dll`、`libtiff-5.dll`、`libwebp-7.dll`、`SDL2.dll`、`SDL2_image.dll`、`SDL2_ttf.dll`、`video_render.fxo`。
    美颜|<ul><li>美颜库：`NERtcBeauty.dll` <li>人脸检测库：`NERtcFaceDetect.dll` <li>机器学习库：`NERtcnn.dll` 
    虚拟背景 |<ul><li>虚拟背景库：`NERtcPersonSegment.dll`  <li>机器学习库：`NERtcnn.dll`
    音频 AI 降噪 |AI 降噪库：`NERtcAiDenoise.dll`
    AI 啸叫检测 | <ul><li>AI 啸叫检测库：`NERtcAiHowling.dll`<li>机器学习库：`NERtcnn.dll`
    人脸增强 | <ul><li>人脸增强库：`NERtcFaceEnhance.dll`<li>机器学习库：`NERtcnn.dll`
    视频降噪 |视频降噪库：`NERtcVideoDenoise.dll`
    视频 AI 超分 | 视频 AI 超分库：`NERtcSuperResolution.dll`
    屏幕增强 | <ul><li>屏幕增强库：`NERtcScreenShareEnhance.dll`<li>机器学习库：`NERtcnn.dll`
    空间音效 |空间音效库：`NERtcAudio3D.dll` |
    
    ::: note note
    - V4.6.20 及之后版本支持插件化方式按需引入动态库。
    - 若您集成的 NERTC SDK 为 V4.6.20 之前的版本，请直接将解压之后的 `NEFundation_Mac.framework` 和 `nertc_sdk_Mac.framework` 文件加入到工程路径下。
    :::



### 步骤3 修改工程配置
1. 在 Microsoft Visual Studio 右侧的**解决方案字样管理器**区域，右键单击目标项目名称，选择**属性**。

2. 将 **api** 文件夹添加到工程项目的 **INCLUDE** 目录下。

    在左侧导航栏中选择**配置属性** > **C/C++** > **常规**，在**附加包含目录**中，添加 api 文件的相对路径，例如：`$(ProjectDir)\nertc_sdk\api`。
    ::: note note
    路径中的 `nertc_sdk` 请替换为实际的 NERTC SDK 文件夹名称。
    :::

    ![附加包含目录.png](https://yx-web-nosdn.netease.im/common/33035ca2c68bd3282994ad7a30172e0f/附加包含目录.png)

  

4. 将 **lib** 文件夹添加到工程项目的 **LIB** 目录下。

    在左侧导航栏中选择**配置属性** > **链接器** > **常规**，在**附加库目录**中，添加 lib\x86 或 lib\x64 的相对路径，例如：`$(ProjectDir)\nertc_sdk\lib\x64`。


    ![附加库目录.png](https://yx-web-nosdn.netease.im/common/19691a36194e4c6ab149694a6f89ead3/附加库目录.png)



  

5. 指定 **nertc_sdk.lib** 到项目的链接。

    在左侧导航栏中选择**配置属性** > **链接器** > **输入**，在**附加依赖项**中，输入 **nertc_sdk.lib**。


    ![附加依赖项.png](https://yx-web-nosdn.netease.im/common/e0734ec4b3dd391d0d18dd4c6a0d2796/附加依赖项.png)

6. 将 **dll** 文件夹下的文件复制到工程可执行文件所在的目录下。

    在左侧导航栏中选择**生成事件** > **生成后事件** > **命令行**，添加拷贝命令 `copy /y $(ProjectDir)\nertc_sdk\dll\x64\*  $(OutDir)` 或  `copy /y $(ProjectDir)\nertc_sdk\dll\x86\*  $(OutDir)`。在编译完成后，自动将 SDK  **dll** 文件夹下的所有文件拷贝到程序的运行目录下。

    ![copy命令.png](https://yx-web-nosdn.netease.im/common/e5fda3fe058a7da23445558f2af981b0/copy命令.png)


  
    ::: note note

    - 自 V4.6.20 版本起，`/dll` 目录下的动态库包括美颜等可选库，如果您的业务对包体积大小有要求，请按需拷贝到对应的动态库，具体请参考 [SDK 目录](#SDK目录)。
    - 修改工程配置中的步骤需要分别设置 Debug 和 Release 模式下的配置。请在**配置**菜单中分别选择**活动（Debug）**和**发布（Release）**，重复以上配置。
    :::

### 步骤4 执行编译

右键单击项目名称，选择**生成**。


## <span id="后续步骤">后续步骤</span>

<a href="https://doc.yunxin.163.com/nertc/quick-start/DYzMjkzMTQ" target="_blank">实现音视频通话</a>
