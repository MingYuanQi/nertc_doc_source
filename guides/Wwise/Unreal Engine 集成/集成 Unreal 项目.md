本文介绍如何在 Audiokinetic Wwise 游戏应用设计工具中，为 Unreal Engine 工程安装配置网易云信音视频通话 Wwise 插件（简称 NERTC Wwise 插件）。

## 开发环境要求

- Wwise SDK 2022.1.8.8316
- Unreal Engine 4.26+
- iOS 10.0+
- Android 5+、Android SDK API 21+
- Windows 7+

## 第一步：安装插件

1. 前往下载 [NERTC Wwise SDK](https://doc.yunxin.163.com/nertc/resource?platform=wwise)，解压后获取到 NERTC Wwise 插件。
2. 打开 **AudioKinetic Launcher > Plugin-ins** 选项，在右侧页面选择 **Add from archive**，选择下载的插件包文件进行安装。
3. 等待安装完成后，可以打开 Wwise 应用设计音效，可参考 [在 Wwise 中安装和使用 ](https://doc.yunxin.163.com/nertc/guide/TMwMzI3NjE?platform=wwise)。

## 第二步：集成 Unreal Engine 项目

1. 打开 Audiokinetic Launcher 应用，选择左侧 Unreal Engine 标签页后，可以看到想要集成的 Unreal Engine 项目。选择该工程，并单击 **Intergate Wwise into Project** 按钮集成到到此 Unreal Engine 项目。如果您的工程已经集成过了，可以忽略此步骤。

2. 将 **{Wwise-INSTALL-ROOT}/SDK/plugins/nertc/Unreal/GameVoiceWwiseDemo/Plugins/NertcPlugin** 复制到您的 Unreal Engine 工程的 **Plugins** 目录，此压缩包已经包含了 Android、iOS、Windows(vc160)、Mac 等平台的库文件，可以直接使用。

    :::note note
    只能存在一份 **WwiseRtcSdk** 插件的副本，所以您需要将 `Plugins/Wwise/ThirdParty` 插件目录下删除 `WwiseRtcSdk`、`nertc-c-sdk` 所有相关的文件。另外，还要从 AkAudio 插件移除 **WwiseRtcSdk** 插件注册。为此，请从 `Plugins/Wwise/Source/AkAudio/Private/Generated` 目录移除所有 `#include <AK/Plugin/WwiseRtcSdkFactory.h>` 代码行。在集成时，会自动添加此文件包含。**为了简化上述操作，可以在执行步骤 1 前，先不要安装 **WwiseRtcSdk** 插件包**。
    :::

3. 增加在您的项目的 `xxx.build.cs` 中对 `NertcPlugin` 的依赖。

   参考示例如下：

   ```C++
   PublicDependencyModuleNames.AddRange(new [] { "Core", "CoreUObject", "Engine", "InputCore", "SlateCore", "UMG", "HeadMountedDisplay", "AkAudio", "NertcPlugin" });
   ```

4. 增加音频权限设置

    :::::: div custom-tabs
    ::: tab Android
    在初始化引擎之前，您需要先初始化 Java 环境。另外，部分权限必须要动态申请权限才能使用，例如蓝牙连接权限，音频权限等。

    示例代码如下：

    ```Java
    #if !WITH_EDITOR && PLATFORM_ANDROID
        //初始化 java 环境
        JNIEnv* env = FAndroidApplication::GetJavaEnv();
        jclass classActivity = FAndroidApplication::FindJavaClass("com/netease/yunxin/lite/unity/UnityUtility");
        jmethodID methodinit = env->GetStaticMethodID(classActivity, "initialize", "(Landroid/content/Context;)V");
        jobject activity = FAndroidApplication::GetGameActivityThis();
        env->CallStaticVoidMethod(classActivity, methodinit, activity);

        //动态请求音频和蓝牙权限
        TArray<FString> AndroidPermission;
        AndroidPermission.Add(FString("android.permission.RECORD_AUDIO"));
        AndroidPermission.Add(FString("android.permission.BLUETOOTH_CONNECT"));
        UAndroidPermissionFunctionLibrary::AcquirePermissions(AndroidPermission);
    #endif // !WITH_EDITOR && PLATFORM_ANDROID
    ```

    :::
    ::: tab iOS

    在 Unreal Engine 工程的 **Project Settings** 窗口 (**Edit** > **Project Settings**) 中，转到 iOS 平台设置。为 Plist Data 添加以下话筒权限：

    ```XML
    <key>NSMicrophoneUsageDescription</key><string>use your microphone</string>
    ```

    如果没有设置音频权限，会导致 App 运行时崩溃。

    :::

    :::
    ::: tab macOS

    在最近的 Unreal Editor 版本中，无法为 macOS 平台设置话筒权限。因此，必须在最终 App 数据包中手动添加话筒权限才能录音。为此，请使用文本编辑器打开 App 数据包中的 `Info.plist` 文件中增加 `Privacy-Microphone Usage Description ` 字段的描述。

    :::
    ::::::

## 下一步

完成上述配置后，您可以参考 [在 Wwise 中的安装和使用](https://doc.yunxin.163.com/nertc/guide/TMwMzI3NjE?platform=wwise) 创建 Wwise Event 等来实现相应的功能。