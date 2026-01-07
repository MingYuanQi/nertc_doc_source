<!--keywords: 实时音视频,音视频通话,webrtc,集成RTC,集成 SDK -->

本文为您介绍了 macOS 端集成网易云信音视频通话 2.0 NERTC SDK 的操作步骤，帮助您快速集成 SDK 并实现实时音视频通话的基本功能。

## <span id="前提条件">准备工作</span>

在开始运行工程之前，请您准备以下开发环境：

- Xcode 11 及以上版本。
- mac OS X 10.11 或以上版本的 Mac 设备。
- 项目已配置有效的开发者签名。

<a id="access"></a>

## 获取 SDK

网易云信音视频通话 2.0 NERTC SDK 提供了以下三种方式获取 SDK，版本更新内容请参考 [更新日志](https://doc.yunxin.163.com/nertc/concept/zU2MjIxNTc?platform=client)：

- **方式一**：您可以通过 CocoaPods 便捷获取 SDK。集成方式请参考下文 [CocoaPods 集成](#CocoaPods)。
- **方式二**：您可以在 [SDK 下载中心](https://doc.yunxin.163.com/nertc/sdk-download) 获取最新的 NERTC SDK 并查看版本号。集成方式请参考下文 [手动集成](#download)。
- **方式三**：若要使用其他版本，例如特殊版本或者定制版本，请 [提交工单](https://app.yunxin.163.com/global/service/ticket/create) 联系网易云信技术支持工程师获取对应的版本号。

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
NERtcnn.xcframework | `Nenn` | 神经网络库<ul><li>4.6.20 ~4.6.X 版本：为必选基础库。</li><li> 5.3.0 及之后版本：<ul><li>集成美颜、虚拟背景、音频啸叫检测、音频 AI 降噪、人脸增强和屏幕增强插件时，为必选基础库。</li><li>集成基础音视频，该动态库可选。</li></ul> | - | 4.6.20 起提供 |
NERtcBeauty.xcframework | `Beauty` | 美颜库（美颜相关），需搭配 `NERtcnn.xcframework` 使用 | - | 4.6.20 起提供 |
NERtcFaceDetect.xcframework | `FaceDetect` | 人脸检测库（美颜相关），需搭配 `NERtcnn.xcframework` 使用 | - | 4.6.20 起提供 |
NERtcPersonSegment.xcframework | `Segment` | 背景分割库，需搭配 `NERtcnn.xcframework` 使用，M1 芯片的 Mac 设备暂不支持此功能 | - | 4.6.20 起提供 |
NERtcAiDenoise.xcframework | `AiDenoise` | 音频 AI 降噪库，需搭配 `NERtcnn.xcframework` 使用 | - | 4.6.40 起提供 |
NERtcAiHowling.xcframework | `AiHowling` | AI 啸叫检测库，需搭配 `NERtcnn.xcframework` 使用，M1 芯片的 Mac 设备暂不支持此功能 | - | 4.6.40 起提供 |
NERtcFaceEnhance.xcframework | `FaceEnhance` | 人脸增强库，需搭配 `NERtcnn.xcframework` 使用 | - | 5.3.0 起提供 |
NERtcVideoDenoise.xcframework | `VideoDenoise` | 视频降噪库 | - | 5.3.0 起提供 |
NERtcSuperResolution.xcframework | `SuperResolution` | 视频 AI 超分库 | - | 5.3.0 起提供 |
NERtcScreenShareEnhance.xcframework | `ScreenShareEnhance` | 屏幕增强库 | - | 5.3.0 起提供 |
NERtcAudio3D.xcframework | `SpatialSound` | 空间音效插件 | - | 5.4.0 起提供 |
:::
::: code 版本低于 5.6.50

文件/文件夹名称 | `subspecs` 值 | 功能/插件名 | <div style="width:50px;">必选？</div> | 版本变化 |
--- | --- | --- | --- | --- |
nertc_sdk_Mac.framework | `RtcBasic` | 音视频库（必选基础库） | ✔️ | 一直提供 |
~~NEFundation_Mac.framework~~ | ~~`RtcBasic`~~ | ~~基础模块库（必选基础库）~~ | ~~✔️~~ | ~~5.3.0 起不再提供~~ |
NERtcnn.framework | `Nenn` | 神经网络库<ul><li>4.6.20 ~4.6.X 版本：为必选基础库。</li><li> 5.3.0 及之后版本：<ul><li>集成美颜、虚拟背景、音频啸叫检测、音频 AI 降噪、人脸增强和屏幕增强插件时，为必选基础库。</li><li>集成基础音视频，该动态库可选。</li></ul> | - | 4.6.20 起提供 |
NERtcBeauty.framework | `Beauty` | 美颜库（美颜相关），需搭配 `NERtcnn.framework` 使用 | - | 4.6.20 起提供 |
NERtcFaceDetect.framework | `FaceDetect` | 人脸检测库（美颜相关），需搭配 `NERtcnn.framework` 使用 | - | 4.6.20 起提供 |
NERtcPersonSegment.framework | `Segment` | 背景分割库，需搭配 `NERtcnn.framework` 使用，M1 芯片的 Mac 设备暂不支持此功能 | - | 4.6.20 起提供 |
NERtcAiDenoise.framework | `AiDenoise` | 音频 AI 降噪库，需搭配 `NERtcnn.framework` 使用 | - | 4.6.40 起提供 |
NERtcAiHowling.framework | `AiHowling` | AI 啸叫检测库，需搭配 `NERtcnn.framework` 使用，M1 芯片的 Mac 设备暂不支持此功能 | - | 4.6.40 起提供 |
NERtcFaceEnhance.framework | `FaceEnhance` | 人脸增强库，需搭配 `NERtcnn.framework` 使用 | - | 5.3.0 起提供 |
NERtcVideoDenoise.framework | `VideoDenoise` | 视频降噪库 | - | 5.3.0 起提供 |
NERtcSuperResolution.framework | `SuperResolution` | 视频 AI 超分库 | - | 5.3.0 起提供 |
NERtcScreenShareEnhance.framework | `ScreenShareEnhance` | 屏幕增强库 | - | 5.3.0 起提供 |
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

## <span id="CocoaPods">方式一：CocoaPods 集成</span>

::: note notice
4.4.1 ~ 4.6.13 版本的 NERTC SDK，暂不支持 CocoaPods 集成，建议使用 [方式二：手动集成](#download)。
:::

1. 请确保您的 Mac 已经安装 Ruby 环境。

2. 安装 CocoaPods。安装 CocoaPods 的详细操作说明请参考 [CocoaPods 官方文档](https://guides.cocoapods.org/using/getting-started.html#getting-started)。

    在终端窗口中输入如下命令可以安装 CocoaPods：

    ```
    sudo gem install cocoapods
    ```

3. 创建 Podfile 文件。

    从 Terminal 中进入您所创建项目所在路径，运行以下命令创建 `Podfile` 文件。

    ```
    pod init
    ```

3. 您可以按需选择直接使用 Podfile 集成 NERtcSDK，或在您的项目组件的 Podspec 中引入 NERtcSDK。

    :::::: div linked-codes
    ::: code 方式一：编辑 Podfile 文件

    ```CocoaPods
    # platform :macos, '10.11'
    target '{YourApp}' do

        pod 'NERtcSDK-MacOS', '~> {version}'
    end
    ```

    参数 | 说明 |
    ---- | ---- |
    YourApp | 您的 Target 名称。
    version | 待集成的 NERTC SDK 版本号。具体的版本号，请参考上文 [获取 SDK](#access)。

    :::
    ::: code 方式二：编辑 Podspec 文件

    ```CocoaPods
    Pod::Spec.new do |{YourSpec}|
        {YourSpec}.dependency "NERtcSDK", "~> {version}"
    end
    ```

    参数 | 说明 |
    ---- | ---- |
    YourSpec | 您的 Target 名称。
    version | 待集成的 NERTC SDK 版本号。具体的版本号，请参考上文 [获取 SDK](#access)。
    :::
    ::::::

    **若您对包体积大小有要求**，请集成 **4.6.20** 及之后版本的 NERTC SDK，实现以插件化的方式按需集成动态库。请参考以下代码实现：

    :::::: div linked-codes
    ::: code 方式一：编辑 Podfile 文件

    ```CocoaPods
    //集成 SDK 但不使用任何插件
    # platform :macos, '10.11'
    target '{YourApp}' do

        pod 'NERtcSDK-MacOS', '~> {version}', :subspecs => ['RtcBasic']
    end
    ```
    ```CocoaPods
    //4.6.20~4.6.X 版本，集成 SDK 且使用部分插件，以美颜模块为例
    # platform :macos, '10.11'
    target '{YourApp}' do

        pod 'NERtcSDK-MacOS', '~> {version}', :subspecs => ['RtcBasic', 'Beauty', 'FaceDetect']
    end
    ```
    ```CocoaPods
    //5.3.0 及以上版本，集成 SDK 且使用部分插件，以美颜模块为例
    # platform :macos, '10.11'
    target '{YourApp}' do

        pod 'NERtcSDK-MacOS', '~> {version}', :subspecs => ['RtcBasic', 'Nenn', 'Beauty', 'FaceDetect']
    end
    ```
    ```CocoaPods
    //5.3.0 及以上版本，集成 SDK 且使用部分插件，以音频啸叫检测插件为例
    # platform :macos, '10.11'
    target '{YourApp}' do

        pod 'NERtcSDKAudio', '~> {version}', :subspecs => ['RtcBasic', 'Nenn', 'AiHowling']
    end
    ```
    以上代码中，`subspecs` 字段请填入待引入的动态库对应的值，取值说明请参考 [SDK 动态库列表](#libs)。
    :::
    ::: code 方式二：编辑 Podspec 文件

    ```CocoaPods
    //集成 SDK 但不使用任何插件
    Pod::Spec.new do |{YourSpec}|
        {YourSpec}.dependency "NERtcSDK-MacOS/RtcBasic", "~> {version}"
    end
    ```
    ```CocoaPods
    //4.6.20~4.6.X 版本，集成 SDK 且使用部分插件，以美颜为例
    Pod::Spec.new do |{YourSpec}|
        {YourSpec}.dependency "NERtcSDK-MacOS/RtcBasic", "~> {version}"
        {YourSpec}.dependency "NERtcSDK-MacOS/Beauty", "~> {version}"
        {YourSpec}.dependency "NERtcSDK-MacOS/FaceDetect", "~> {version}"
    end
    ```
    ```CocoaPods
    //5.3.0 及以上版本，集成 SDK 且使用部分插件，以美颜为例
    Pod::Spec.new do |{YourSpec}|
        {YourSpec}.dependency "NERtcSDK-MacOS/RtcBasic", "~> {version}"
        {YourSpec}.dependency "NERtcSDK-MacOS/Nenn", "~> {version}"
        {YourSpec}.dependency "NERtcSDK-MacOS/Beauty", "~> {version}"
        {YourSpec}.dependency "NERtcSDK-MacOS/FaceDetect", "~> {version}"
    end
    ```
    ```CocoaPods
    //5.3.0 及以上版本，集成 SDK 且使用部分插件，以音频啸叫检测插件为例
    Pod::Spec.new do |{YourSpec}|
        {YourSpec}.dependency "NERtcSDKAudio/RtcBasic", "~> {version}"
        {YourSpec}.dependency "NERtcSDKAudio/Nenn", "~> {version}"
        {YourSpec}.dependency "NERtcSDKAudio/AiHowling", "~> {version}"
    end
    ```
    以上代码中，`dependency` 字段请填入待引入的动态库对应的值，取值说明请参考 [SDK 动态库列表](#libs)。
    :::
    ::::::

5. 执行以下命令查询本地库版本。

    ```
    pod search NERtcSDK-MacOS
    ```

6. 若不是最新版本，可以执行以下命令更新本地库版本。

    ```
    pod repo update
    ```

7. 执行以下命令安装 SDK。

    ```
    pod install
    ```

8. 设置签名和添加设备权限，具体请参考 [设置签名并添加媒体设备权限](#设置签名并添加媒体设备权限)。

## <span id="download">方式二：手动集成</span>

1. 前往 [网易云信 SDK 下载中心](https://doc.yunxin.163.com/nertc/sdk-download) 下载 SDK。

3. 将解压之后的 `xcframework` 后缀的文件加入到工程目录下。

    <!-- <img alt="Xnip2023-04-12_09-52-01.jpg" src="https://yx-web-nosdn.netease.im/common/c6e51d39644d7b4f080ae204cd41bc6e/Xnip2023-04-12_09-52-01.jpg" style="width:80%;border: 1px solid #BFBFBF;"> -->

    ::: note note
    - 不同版本的动态库有差异，请以实际为准，具体请参考 [SDK 目录](#access)。
    - **若您对包体积大小有要求**，请集成 **4.6.20** 及之后版本的 NERTC SDK，您可以通过插件化的方式按需集成动态库。
    :::

3. 打开 Xcode，在左侧导航栏中选择目标 Target，单击 **Build Phases** 页签，在 **Link Binary with Libraries** 区域，单击 **+** 添加相应的 xcframework 动态库，待添加的动态库请参考上文 [动态库](#libs)。

    <!-- 
    <img alt="添加动态库.png" src="https://yx-web-nosdn.netease.im/common/63454717aed9d1b1309d5e0ed717a8c4/添加动态库.png" style="width:80%;border: 1px solid #BFBFBF;"> -->

4. 将 **Embed** 属性设置为 **Embed & Sign**，保证 SDK 动态库和应用签名保持一致。

    <img alt="添加动态库结果.png" src="https://yx-web-nosdn.netease.im/common/61412fb3b16293d9c4ca188e525a5f46/image.png" style="width:80%;border: 1px solid #BFBFBF;">

5. 设置签名和添加设备权限，具体请参考 [设置签名并添加媒体设备权限](#设置签名并添加媒体设备权限)。

    ::: note notice
    在手动导入 SDK 的情况下，由于 SDK 包含模拟器版本，会导致打包失败。所以需要在打包之前将模拟器版本剥去，具体步骤参考 [应用打包前剥离模拟器版本](#应用打包前剥离模拟器版本)。
    :::

## <span id="设置签名并添加媒体设备权限">设置签名并添加媒体设备权限</span>

1. 使用 SDK 提供的音视频功能，需要给 App 授权麦克风、摄像头和 Wi-Fi 的使用权限。

    在 `info.plist` 中添加以下三项：

    - **Privacy - Microphone Usage Description**，并填入麦克风使用目的提示语。
    - **Privacy - Camera Usage Description**，并填入摄像头使用目的提示语。
    - **Application uses Wi-Fi**，设置为 **YES**。

        <img alt="授权.png" src="https://yx-web-nosdn.netease.im/common/dff095b997d372f4f67b128e2e682652/授权.png" style="width:80%;border: 1px solid #BFBFBF;">

2. 设置签名。

    1. 在 Xcode 中，依次选择 **TARGETS** > **Project Name**，单击 **Signing & Capabilities** 页签，勾选 **Automatically manage signing**。

        <img alt="Xcode_Signing.png" src="https://yx-web-nosdn.netease.im/common/0bf7780b842432452a0133d4e576b65a/Xcode_Signing.png" style="width:80%;border: 1px solid #BFBFBF;">

    2. 在 **Team** 中选择您的开发团队。

3. 如果 App 启用了 **App Sandbox** 或 **Hardened Runtime**，请在 **Signing & Capabilities** 页面的 **App Sandbox** 和 **Hardened Runtime** 区域，选择 **Network**、**Camera** 和 **Audio Input** 这几个选项。

    <img alt="AppSandbox.png" src="https://yx-web-nosdn.netease.im/common/ccbb5b6c9a3e44798203cc4f3539d757/AppSandbox.png" style="width:80%;border: 1px solid #BFBFBF;">

    <img alt="ResourceAccess.png" src="https://yx-web-nosdn.netease.im/common/cb70778ce1f6d59cfd5dff20629096ef/ResourceAccess.png" style="width:80%;border: 1px solid #BFBFBF;">

## <span id="后续步骤">下一步</span>

[实现音视频通话](https://doc.yunxin.163.com/nertc/guide/TU1OTc0NzM?platform=macOS)。