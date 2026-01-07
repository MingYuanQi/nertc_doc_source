<!--keywords: 音频通话,webrtc,集成RTC,集成 SDK -->

本文为您介绍了 macOS 端集成网易云信音视频通话 2.0 NERTC SDK 的操作步骤，帮助您快速集成 SDK 并实现实时音频通话的基本功能。

## <span id="前提条件">准备工作</span>

在开始运行工程之前，请您准备以下开发环境：

- Xcode 11 及以上版本。
- mac OS X 10.11 或以上版本的 Mac 设备。
- 项目已配置有效的开发者签名。

<a id="access"></a>

## 获取 SDK

网易云信音视频通话 2.0 NERTC SDK 提供了以下两种方式获取纯音频 SDK，版本更新内容请参考 [更新日志](https://doc.yunxin.163.com/nertc/concept/zU2MjIxNTc?platform=client)：

- **方式一**：您可以在 [SDK 下载中心](https://doc.yunxin.163.com/nertc/sdk-download) 获取最新的 NERTC SDK 并查看版本号。集成方式请参考下文 [手动集成](#download)。
- **方式二**：若要使用其他版本，例如特殊版本或者定制版本，请 [提交工单](https://app.yunxin.163.com/global/service/ticket/create) 联系网易云信技术支持工程师获取对应的版本号。

**若您对包体积大小有要求**，推荐您集成 **4.6.20** 及之后版本的 NERTC SDK，实现以插件化的方式按需集成动态库。

<span id="libs"></span>

## 动态库

SDK 目录中的每一个 `xcframework` 都代表着动态库，包括基础库和特色库。以插件化的方式按需集成动态库，有助于您减轻包体积。例如：

- 您只集成音视频能力可以只接入 `nertc_sdk_Mac.xcframework`。
- 您要实现美颜功能则可以只接入 `nertc_sdk_Mac.xcframework`、`NERtcBeauty.xcframework`、`NERtcFaceDetect.xcframework`、`NERtcnn.xcframework`。

NERTC SDK 详细的动态库介绍如下表所示：

:::::: div linked-codes
::: code 5.6.50 起

<note type="note">从 5.6.50 版本起，NERTC SDK 支持跨平台开发的 XCFramework 框架格式。SDK 目录下的 `.framework` 文件后缀名修改为 `.xcframework`。如果您采用 [手动集成](#download) 方式集成 NERTC SDK，并从低版本升级至 5.6.50 及以上，请重新添加 XCFramework 依赖。</note>

文件/文件夹名称 | `subspecs` 值 | 功能/插件名 | <div style="width:50px;">必选？</div> | 版本变化 |
--- | --- | --- | --- | --- |
nertc_sdk_Mac.xcframework | `RtcBasic` | 音视频库（必选基础库） | ✔️ | 一直提供 |
NERtcnn.xcframework | `Nenn` | 神经网络库<ul><li>4.6.20 ~4.6.X 版本：为必选基础库。</li><li> 5.3.0 及之后版本：<ul><li>集成美颜、虚拟背景、音频啸叫检测、人脸增强和屏幕增强插件时，为必选基础库。</li><li>集成基础音视频，该动态库可选。</li></ul> | - | 4.6.20 起提供 |
NERtcAiDenoise.xcframework | `AiDenoise` | 音频 AI 降噪库，需搭配 `NERtcnn.xcframework` 使用 | - | 4.6.40 起提供 |
NERtcAiHowling.xcframework | `AiHowling` | AI 啸叫检测库，需搭配 `NERtcnn.xcframework` 使用，M1 芯片的 Mac 设备暂不支持此功能 | - | 4.6.40 起提供 |
NERtcAudio3D.xcframework | `SpatialSound` | 空间音效插件 | - | 5.4.0 起提供 |

:::
::: code 版本低于 5.6.50
文件/文件夹名称 | `subspecs` 值 | 功能/插件名 | <div style="width:50px;">必选？</div> | 版本变化 |
--- | --- | --- | --- | --- |
nertc_sdk_Mac.framework | `RtcBasic` | 音视频库（必选基础库） | ✔️ | 一直提供 |
NERtcnn.framework | `Nenn` | 神经网络库<ul><li>4.6.20 ~4.6.X 版本：为必选基础库。</li><li> 5.3.0 及之后版本：<ul><li>集成美颜、虚拟背景、音频啸叫检测、人脸增强和屏幕增强插件时，为必选基础库。</li><li>集成基础音视频，该动态库可选。</li></ul> | - | 4.6.20 起提供 |
NERtcAiDenoise.framework | `AiDenoise` | 音频 AI 降噪库，需搭配 `NERtcnn.framework` 使用 | - | 4.6.40 起提供 |
NERtcAiHowling.framework | `AiHowling` | AI 啸叫检测库，需搭配 `NERtcnn.framework` 使用，M1 芯片的 Mac 设备暂不支持此功能 | - | 4.6.40 起提供 |
NERtcAudio3D.framework | `SpatialSound` | 空间音效插件 | - | 5.4.0 起提供 |
:::
::::::

## 创建项目

::: details 本步骤是可选步骤，如果您需要集成到已有的项目，请忽略该步骤。单击展开查看创建 XCode 新项目的步骤。

1. 在 XCode 里，依次选择 **Create a New XCode Project** > **Single View App** > **Next** 新建工程。

    <img style="width:50%;border: 1px solid #BFBFBF;" src="https://yx-web-nosdn.netease.im/common/b7e79e40feefe5ecab0d04930b87af43/ios_new_project.png" alt="image" />
2. 配置工程相关信息和合适的工程本地路径，单击 **Next**。

    <img style="width:50%;border: 1px solid #BFBFBF;" src="https://yx-web-nosdn.netease.im/common/2deb73bd6f161a81e2f6e849a21ab2a4/ios_configure_project.png" alt="image" />
:::

## <span id="download">手动集成</span>

1. 前往 [网易云信 SDK 下载中心](https://doc.yunxin.163.com/nertc/sdk-download) 下载 SDK。

3. 将解压之后的 `xx.xcframework` 后缀的文件加入到工程目录下。

    <!-- <img alt="Xnip2023-04-12_09-52-01.jpg" src="https://yx-web-nosdn.netease.im/common/c6e51d39644d7b4f080ae204cd41bc6e/Xnip2023-04-12_09-52-01.jpg" style="width:80%;border: 1px solid #BFBFBF;"> -->

    ::: note note
    - 不同版本的动态库有差异，请以实际为准，具体请参考 [SDK 目录](#access)。
    - **若您对包体积大小有要求**，请集成 **4.6.20** 及之后版本的 NERTC SDK，您可以通过插件化的方式按需集成动态库。
    - 若您集成的 NERTC SDK 为 V4.6.20 之前的版本，请直接将解压之后的 `NEFundation_Mac.xcframework` 和 `nertc_sdk_Mac.xcframework` 文件加入到工程路径下。
    :::

3. 打开 Xcode，在左侧导航栏中选择目标 Target，单击 **Build Phases** 页签，在 **Link Binary with Libraries** 区域，单击 **+** 添加相应的 xcframework 动态库，待添加的动态库请参考上文 [动态库](#libs)。

    <!-- <img alt="添加动态库.png" src="https://yx-web-nosdn.netease.im/common/6c914e261ca776398289f917f7bca097/添加动态库-纯音频.png" style="width:80%;border: 1px solid #BFBFBF;"> -->

4. 将 **Embed** 属性设置为 **Embed & Sign**，保证 SDK 动态库和应用签名保持一致。

    <img alt="添加动态库结果.png" src="https://yx-web-nosdn.netease.im/common/c47ba88e7c5a54e58c407e1e03b7a89e/image.png" style="width:80%;border: 1px solid #BFBFBF;">

5. 设置签名和添加设备权限，具体请参考 [设置签名并添加媒体设备权限](#设置签名并添加媒体设备权限)。

    ::: note notice
    在手动导入 SDK 的情况下，由于 SDK 包含模拟器版本，会导致打包失败。所以需要在打包之前将模拟器版本剥去，具体步骤参考 [应用打包前剥离模拟器版本](#应用打包前剥离模拟器版本)。
    :::

## <span id="设置签名并添加媒体设备权限">设置签名并添加媒体设备权限</span>

1. 使用 SDK 提供的音视频功能，需要给 App 授权麦克风、摄像头和 Wi-Fi 的使用权限。

    在 `info.plist` 中添加以下三项：

    - **Privacy - Microphone Usage Description**，并填入麦克风使用目的提示语。
    - **Application uses Wi-Fi**，设置为 **YES**。

        <img alt="授权.png" src="https://yx-web-nosdn.netease.im/common/dd284a7b4b39e0ab3bf74162d38f4327/授权麦克风权限-纯音频.png" style="width:80%;border: 1px solid #BFBFBF;">

2. 设置签名。

    1. 在 Xcode 中，依次选择 **TARGETS** > **Project Name**，单击 **Signing & Capabilities** 页签，勾选 **Automatically manage signing**。

        <img alt="Xcode_Signing.png" src="https://yx-web-nosdn.netease.im/common/0bf7780b842432452a0133d4e576b65a/Xcode_Signing.png" style="width:80%;border: 1px solid #BFBFBF;">

    2. 在 **Team** 中选择您的开发团队。

3. 如果 App 启用了 **App Sandbox** 或 **Hardened Runtime**，请在 **Signing & Capabilities** 页面的 **App Sandbox** 和 **Hardened Runtime** 区域，选择 **Network** 和 **Audio Input** 这几个选项。

    <img alt="AppSandbox.png" src="https://yx-web-nosdn.netease.im/common/eb5e44486fad21cc5b9edc593227d214/AppSandbox-纯音频.png" style="width:80%;border: 1px solid #BFBFBF;">

    <img alt="ResourceAccess.png" src="https://yx-web-nosdn.netease.im/common/566ed3daf5b80d63f2beda2474be1c27/ResourceAccess-纯音频.png" style="width:80%;border: 1px solid #BFBFBF;">

## <span id="后续步骤">下一步</span>

[实现音视频通话](https://doc.yunxin.163.com/nertc/guide/TU1OTc0NzM?platform=macOS)。