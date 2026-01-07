<!--和互动直播对应文档基本相同，替换产品名称后即可重用-->

本文为您介绍了 iOS 端集成网易云信音视频通话 2.0 NERTC SDK 的操作步骤，帮助您快速集成 SDK 并实现实时纯音频通话的基本功能。

## <span id="前提条件">准备工作</span>

在开始运行工程之前，请您准备以下开发环境：

- Xcode 10 及以上版本。
- iOS 9.0 及以上版本的 iOS 设备。

<a id="access"></a>

## 第一步：获取 SDK

网易云信音视频通话 2.0 NERTC SDK 提供了以下三种方式获取 SDK，版本更新内容请参考 [更新日志](https://doc.yunxin.163.com/nertc/concept/TcyMzY0ODg?platform=client)：

- **方式一**：您可以通过 CocoaPods 便捷获取 SDK。集成方式请参考下文 [CocoaPods 集成](#CocoaPods)。
- **方式二**：您可以在 [SDK 下载中心](https://doc.yunxin.163.com/nertc/sdk-download) 获取最新的 NERTC SDK 并查看版本号。集成方式请参考下文 [手动集成](#download)。
- **方式三**：若要使用其他版本，例如特殊版本或者定制版本，请 [提交工单](https://app.yunxin.163.com/global/service/ticket/create) 联系网易云信技术支持工程师获取对应的版本号。

**若您对包体积大小有要求**，推荐您集成 **4.6.20** 及之后版本的 NERTC SDK，实现以插件化的方式按需集成动态库。

### SDK 目录

获取了 SDK 后，您会查看到 <span id="SDKtoc">SDK 目录</span>，其中属于纯音频的文件为：

```
iOS
|
|-- NERtcSDK.xcframework                     //推荐放置在 libs/ 目录
|-- NERtcnn.xcframework                      //推荐放置在 libs/ 目录
|-- NERtcReplayKit.xcframework               //推荐放置在 libs/ 目录
|-- NERtcAiDenoise.xcframework               //推荐放置在 libs/ 目录
|-- NERtcAiHowling.xcframework               //推荐放置在 libs/ 目录
|-- NERtcAudio3D.xcframework                 //推荐放置在 libs/ 目录
```

<span id="libs"></span>

### 动态库

SDK 目录中的每一个 `framework` 都代表着动态库，包括基础库和特色库。以插件化的方式按需集成动态库，有助于您减轻包体积。例如：

- 您只集成纯音频能力可以只接入 `NERtcSDK.xcframework`。
- 您要实现音频降噪功能则可以只接入 `NERtcSDK.xcframework`、`NERtcAiDenoise.xcframework`、`NERtcnn.xcframework`。

NERTC SDK 详细的动态库介绍如下表所示：

:::::: div linked-codes
::: code 5.6.50 起

<note type="note">从 5.6.50 版本起，NERTC SDK 支持跨平台开发的 XCFramework 框架格式。SDK 目录下的 `.framework` 文件后缀名修改为 `.xcframework`。如果您采用 [手动集成](#download) 方式集成 NERTC SDK，并从低版本升级至 5.6.50 及以上，请重新添加 XCFramework 依赖。</note>

文件/文件夹名称 | `subspecs` 值 | 功能/插件名 | <div style="width:50px;">必选？</div> | 版本变化 |
--- | --- | --- | --- | --- |
NERtcSDK.xcframework | `RtcBasic` | 音视频库（必选基础库） | ✔ | 一直提供 |
NERtcnn.xcframework | `Nenn` | 神经网络库。  <note type="notice"> 只有集成音频啸叫检测插件和音频降噪插件时，才需要集成该库文件。基础音视频不需要集成该库文件。</note> | - | 4.6.20 起提供 |
NERtcReplayKit.xcframework | `ScreenShare` | 屏幕共享库 | - | 4.6.20 起提供 |
NERtcAiHowling.xcframework | `AiHowling` | AI 啸叫检测库，需搭配 `NERtcnn.xcframework` 使用 | - | 4.6.40 起提供 |
NERtcAiDenoise.xcframework | `AiDenoise` | 音频 AI 降噪库，需搭配 `NERtcnn.xcframework` 使用 | - | 4.6.40 起提供 |
NERtcAudio3D.xcframework | `SpatialSound` | 空间音效插件 | - | 5.4.0 起提供 |

:::
::: code 版本低于 5.6.50
文件/文件夹名称 | `subspecs` 值 | 功能/插件名 | <div style="width:50px;">必选？</div> | 版本变化 |
--- | --- | --- | --- | --- |
NERtcSDK.framework | `RtcBasic` | 音视频库（必选基础库） | ✔ | 一直提供 |
NERtcnn.framework | `Nenn` | 神经网络库。  <note type="notice"> 只有集成音频啸叫检测插件和音频降噪插件时，才需要集成该库文件。基础音视频不需要集成该库文件。</note> | - | 4.6.20 起提供 |
NERtcReplayKit.framework | `ScreenShare` | 屏幕共享库 | - | 4.6.20 起提供 |
NERtcAiHowling.framework | `AiHowling` | AI 啸叫检测库，需搭配 `NERtcnn.framework` 使用 | - | 4.6.40 起提供 |
NERtcAiDenoise.framework | `AiDenoise` | 音频 AI 降噪库，需搭配 `NERtcnn.framework` 使用 | - | 4.6.40 起提供 |
NERtcAudio3D.framework | `SpatialSound` | 空间音效插件 | - | 5.4.0 起提供 |
:::
::::::

## 第二步：创建项目

本步骤是可选步骤，如果您需要集成到已有的项目，请忽略该步骤。

::: details 单击展开查看创建 XCode 新项目的步骤。

1. 在 XCode 里，依次选择 **Create a New XCode Project** > **Single View App** > **Next** 新建工程。

    <img style="width:50%;border: 1px solid #BFBFBF;" src="https://yx-web-nosdn.netease.im/common/b7e79e40feefe5ecab0d04930b87af43/ios_new_project.png" alt="image" />
2. 配置工程相关信息和合适的工程本地路径，单击 **Next**。

    <img style="width:50%;border: 1px solid #BFBFBF;" src="https://yx-web-nosdn.netease.im/common/2deb73bd6f161a81e2f6e849a21ab2a4/ios_configure_project.png" alt="image" />
:::

## 第三步：集成 SDK

### <span id="CocoaPods">方式一：CocoaPods 集成</span>

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
    # platform :ios, '9.0'
    target '{YourApp}' do
        use_frameworks!

        pod 'NERtcSDKAudio', '~> {version}'
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
        {YourSpec}.dependency "NERtcSDKAduio", "~> {version}"
    end
    ```

    参数 | 说明 |
    ---- | ---- |
    YourSpec | 您的 Target 名称。
    version | 待集成的 NERTC SDK 版本号。具体的版本号，请参考上文 [获取 SDK](#access)。
    :::
    ::::::

    实现以插件化的方式按需集成动态库，请参考以下代码实现：

    :::::: div linked-codes
    ::: code 方式一：编辑 Podfile 文件

    ```CocoaPods
    //集成 SDK 但不使用任何插件
    # platform :ios, '9.0'
    target '{YourApp}' do
        use_frameworks!

        pod 'NERtcSDKAduio', '~> {version}', :subspecs => ['RtcBasic']
    end
    ```
    ```CocoaPods
    //5.3.0 及以上版本，集成 SDK 且使用部分插件，以音频啸叫检测插件为例
    # platform :ios, '9.0' 
    target '{YourApp}' do
        use_frameworks!

        pod 'NERtcSDKAudio', '~> {version}', :subspecs => ['RtcBasic', 'Nenn', 'AiHowling']
    end
    ```
    以上代码中，`subspecs` 字段请填入待引入的动态库对应的值，取值说明请参考 [SDK 动态库列表](#libs)。
    :::
    ::: code 方式二：编辑 Podspec 文件

    ```CocoaPods
    //集成 SDK 但不使用任何插件
    Pod::Spec.new do |{YourSpec}|
        {YourSpec}.dependency "NERtcSDKAduio/RtcBasic", "~> {version}"
    end
    ```
    ```CocoaPods
    //5.3.0 及以上版本，集成 SDK 且使用部分插件，以音频啸叫检测插件为例
    Pod::Spec.new do |{YourSpec}|
        {YourSpec}.dependency "NERtcSDKAduioAudio/RtcBasic",  "~> {version}"
        {YourSpec}.dependency "NERtcSDKAduioAudio/Nenn",  "~> {version}"
        {YourSpec}.dependency "NERtcSDKAduioAudio/AiHowling",  "~> {version}"
    end
    ```
    以上代码中，`dependency` 字段请填入待引入的动态库对应的值，取值说明请参考 [SDK 动态库列表](#libs)。
    :::
    ::::::

5. 执行以下命令查询本地库版本。

    ```
    pod search NERtcSDKAudio
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

### <span id="download">方式二：手动集成</span>

1. 前往 [网易云信 SDK 下载中心](https://doc.yunxin.163.com/nertc/sdk-download) 下载 SDK。

3. 将解压之后的 `xcframework` 后缀的文件加入到工程的 `libs/` 目录下。

    <img alt="Xnip2023-04-12_09-52-01.jpg" src="https://yx-web-nosdn.netease.im/common/87f1b0a78102e5668dd18a5159e4d881/image.png" style="width:80%;border: 1px solid #BFBFBF;">

    ::: note note
    - 不同版本的动态库有差异，请以实际为准，具体请参考 [SDK 目录](#SDKtoc)。
    - **若您对包体积大小有要求**，您可以通过插件化的方式按需集成动态库。
    :::

4. 以使用 Xcode 11.5 版本为例，依次选择 **TARGETS > Project Name > General > Frameworks, Libraries, and Embedded Content**，单击 **+**，再单击 **Add Other…**，将上述解压得到的 SDK 文件添加进去。

    ::: note notice
    将 **Embed** 属性设置为 **Embed & Sign**，以使得 SDK 动态库和应用签名保持一致。
    :::

    <img alt="Xnip2023-04-12_09-53-34.jpg" src="https://yx-web-nosdn.netease.im/common/76696c2d4cee491153e37d46da0ec01d/image.png" style="width:80%;border: 1px solid #BFBFBF;">

5. 设置签名和添加设备权限，具体请参考 [设置签名并添加媒体设备权限](#设置签名并添加媒体设备权限)。

    <!-- ::: note notice
    在手动导入 SDK 的情况下，由于 SDK 包含模拟器版本，会导致打包失败。所以需要在打包之前将模拟器版本剥去，具体步骤参考 [应用打包前剥离模拟器版本](#应用打包前剥离模拟器版本)。
    ::: -->

<span id="设置签名并添加媒体设备权限"></span>

## 第四步：设置签名

1. 在 Xcode 中，依次选择 **TARGETS** > **Project Name**，单击 **Signing & Capabilities**，勾选 **Automatically manage signing**。

    <img alt="Xcode_Signing.png" src="https://yx-web-nosdn.netease.im/common/0bf7780b842432452a0133d4e576b65a/Xcode_Signing.png" style="width:80%;border: 1px solid #BFBFBF;">

2. 在 **Team** 中选择您的开发团队。

## 第五步：添加权限

1. 若您的 App 需要在退到后台时仍然运行相关功能，请打开后台音频权限。

    在 **Signing & Capabilities** 页面，将设置项 **Background Modes** 设定为 ON，并勾选 **Audio，AirPlay and Picture in Picture**。

    <img alt="Xnip2022-12-02_16-33-26.jpg" src="https://yx-web-nosdn.netease.im/common/0f9308b5afc5a46984f1a59eaa917e1c/Xnip2022-12-02_16-33-26.jpg" style="width:80%;border: 1px solid #BFBFBF;">

    打开后台音频权限之后，应用在手机后台运行时，SDK 默认在后台也可以继续处理音频流，维持通话。

3. 若您的 App 需要正常使用 SDK 提供的音频功能，请给 App 授权麦克风和 Wi-Fi 的使用权限。

    编辑 `info.plist` 文件，添加以下项。

    - **Privacy - Microphone Usage Description**，并填入麦克风使用目的提示语。
    - **Application uses Wi-Fi**，设置为 **YES**。

        <img alt="Xnip2022-12-02_16-46-19.jpg" src="https://yx-web-nosdn.netease.im/common/a3d9df58a1f491ee44dd3ce7f924312c/Xnip2023-08-08_20-22-37.jpg" style="width:80%;border: 1px solid #BFBFBF;">

## <span id="后续步骤">下一步</span>

[实现音视频通话](https://doc.yunxin.163.com/nertc/quick-start/DA0MzExMTY)。
<!-- 
- <span id="应用打包前剥离模拟器版本">应用打包前剥离模拟器版本</span>（仅手动集成 SDK 时需要）

    1. 在工程里创建 `nim_strip_archs.sh` 脚本到指定目录，例如 `Supporting Files` 目录。
    2. 在 `Build Phases` 中增加过程，类型为 `New Run Script Phase`。需要把去掉模拟器的 Run Script 脚本放在 Embed Frameworks 之后。
    3. 在工程里添加内容: `/bin/sh 您的脚本路径`，例如 `/bin/sh "${SRCROOT}/NIMDemo/Supporting Files/nim_strip_archs.sh"`。
    4. 将如下内容复制到脚本：

        ```Bash
        #!/bin/sh

        # Strip invalid architectures

        strip_invalid_archs() {
        binary="$1"
        echo "current binary ${binary}"
        # Get architectures for current file
        archs="$(lipo -info "$binary" | rev | cut -d ':' -f1 | rev)"
        stripped=""
        for arch in $archs; do
        if ! [[ "${ARCHS}" == *"$arch"* ]]; then
        if [ -f "$binary" ]; then
        # Strip non-valid architectures in-place
        lipo -remove "$arch" -output "$binary" "$binary" || exit 1
        stripped="$stripped $arch"
        fi
        fi
        done
        if [[ "$stripped" ]]; then
        echo "Stripped $binary of architectures:$stripped"
        fi
        }

        APP_PATH="${TARGET_BUILD_DIR}/${WRAPPER_NAME}"

        # This script loops through the frameworks embedded in the application and
        # removes unused architectures.
        find "$APP_PATH" -name '*.framework' -type d | while read -r FRAMEWORK
        do
        FRAMEWORK_EXECUTABLE_NAME=$(defaults read "$FRAMEWORK/Info.plist" CFBundleExecutable)
        FRAMEWORK_EXECUTABLE_PATH="$FRAMEWORK/$FRAMEWORK_EXECUTABLE_NAME"
        echo "Executable is $FRAMEWORK_EXECUTABLE_PATH"

        strip_invalid_archs "$FRAMEWORK_EXECUTABLE_PATH"
        done
        ``` -->

## <span id="常见问题">常见问题</span>

::: details 1. 如何减小集成 SDK 后的 App 包体积？

详情请参考 [减小包体积大小](https://doc.yunxin.163.com/nertc/quick-start/DE1MzgyMzU?platform=iOS)。
:::

::: details 2. 如何用 Cocoapods 同时集成 RTC 与播放器？
RTC 与播放器有共用的基础库，同时集成时，原则为使用 RTC 附带的基础库。

- 若播放器为 3.1.6 及之后版本

    支持用 Cocoapods 同时集成 RTC 与播放器。

    - **方式 1：通过 podfile 集成的示例代码**：

        ```
        target 'App' do
        use_frameworks!

        pod 'NERtcSDK', 'x.x.x'
        pod 'NELivePlayer', '3.1.6', :subspecs => ['LivePlayer']
        end
        ```
    - **方式 2：通过 podspec 集成的示例代码**：
        ```
        Pod::Spec.new do |s|
            s.dependency "NELivePlayer/LivePlayer", "3.1.6"
        end
        ```
- 若播放器为 3.1.6 之前的版本

    不支持用 Cocoapods 同时集成 RTC 与播放器，建议用 Cocoapods 集成 RTC，手动引入播放器并删除其附带的基础库。
:::

::: details 3. pod 集成提示 `The 'Pods-Yaame' target has frameworks with conflicting names: nmcbasicmoduleframework.framework`

**问题原因**

RTC 5.3.0 之前版本，NERtcSDK 基础库包含 `NMCBasicModuleFramework.framework`。如果集成播放器 SDK（NELivePlayer），默认也会包含 `NMCBasicModuleFramework.framework`。通过 pod 引入的话就会报错冲突。

**问题解决**

修改播放器 SDK（NELivePlayer） 的引入方式，只引入 `LivePlayer` 排除掉 `NMCBasicModuleFramework.framework` 库。示例代码如下：

- **方式一：通过 podfile 集成的示例代码**

    ```
    target 'App' do
    use_frameworks!

    pod 'NERtcSDK', 'x.x.x'
    pod 'NELivePlayer', '3.1.6', :subspecs => ['LivePlayer']
    end
    ```

- **方式二：通过 podspec 集成的示例代码**

    ```
    Pod::Spec.new do |s|
        s.dependency "NELivePlayer/LivePlayer", "3.1.6"
    end
    ```
:::