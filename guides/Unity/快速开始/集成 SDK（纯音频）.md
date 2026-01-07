本文为您介绍了 Unity 框架集成 NERTC SDK（纯音频版本）的操作步骤，帮助您快速集成 NERTC SDK 并实现音频通话的基本功能。

## 前提条件

在开始运行工程之前，请您准备以下开发环境：

- 最新版本的 Unity Hub。
- [Unity](https://unity.com/download) 2019.4.2 或以上版本。在安装 Unity 时，请根据需要选择相应的平台，并勾选相应的 **PLATFORMS** 模块进行下载。
- 操作系统与开发环境要求：

:::::: div custom-tabs
::: tab Android

- Android SDK API 等级 19 或以上。
- Android Studio 3.0 或以上版本。
- Android 系统 4.4 或以上版本的移动设备。

:::
::: tab iOS
- Xcode 10 及以上版本。
- iOS 9.0 及以上版本的 iOS 设备。
:::
::: tab Windows
- 开发环境：Microsoft Visual Studio 2017（推荐）或以上版本
- 操作系统：Microsoft Windows 7 或以上版本
- 编译器：Microsoft Visual C++ 2017 或以上版本
:::
::: tab macOS
- Xcode 11 及以上版本。
- mac OS X 10.11 或以上版本的 Mac 设备。
- 项目已配置有效的开发者签名。
:::
::::::

## SDK 目录


| 文件/文件夹名称 |是否必选| 说明 |
| --- | --- |---|
| com.netease.game.rtc-x.x.xx.tgz   |是| 音频库。                                     |
| com.netease.game.rtc.nenn-x.x.xx.tgz  | 否| 神经网络库（自 V5.4.0 起提供，以实现插件化）。  <note type="notice"> 只有集成音频啸叫检测插件时，才需要集成该库文件。基础音视频不需要集成该库文件。</note>     |
| com.netease.game.rtc.ains-x.x.xx.tgz   | 否| 音频 AI 降噪（自 V5.4.0 起提供，以实现插件化）。      |
| com.netease.game.rtc.aihowling-x.x.xx.tgz  | 否| AI 啸叫检测（自 V5.4.0 起提供，以实现插件化）。      |
| com.netease.game.rtc.spatialaudio-x.x.xx.tgz   | 否| 空间音效（自 V5.4.0 起提供，以实现插件化）。      |



## 步骤一 集成 SDK
1. 下载最新版本的 <a href="https://doc.yunxin.163.com/nertc/sdk-download?platform=unity" target="_blank">NERTC Unity SDK</a>，并解压到本地。
2. 把下载到的 SDK 文件放到 `Packages` 目录。
   
   V5.4.0 及之后版本支持插件化集成，请根据您需要集成的能力，选择相应的插件包到 `Packages` 目录。


3. 打开 `Unity Editor` 的 `Package Manager`，单击左上角 `+` 图标，然后单击 `"Add Package from tarball..."`，再选中 `Packages` 目录下的 `com.netease.game.rtc-*.tgz` 文件，即可完成导入。

## 步骤二 添加权限

使用音视频库前，您需要在 Unity 中为工程添加麦克风等设备访问权限。
:::::: div custom-tabs
::: tab iOS

在 Unity 中打开 **Build Settings** > **Player Settings** ，为 **Camera Usage Description** 和 **Microphone Usage Description** 添加描述。

![](https://yx-web-nosdn.netease.im/quickhtml%2Fassets%2Fyunxin%2Fdoc%2FG2-UnitySDK01.png)

:::
::: tab Android

1. 在 Unity 中打开 **Build Settings** > **Player Settings**，在 **Publishing Settings** > **Build** 下勾选 **Custom Main Manifest**。

    系统会自动生成 `AndroidManifest.xml` 文件。

2. 修改 `Assets\Plugins\Android\AndroidManifest.xml` 文件，添加必要的设备权限。


```xml
<!--网络相关-->
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
    <uses-permission android:name="android.permission.CHANGE_WIFI_STATE"/>
    <!--防止通话过程中锁屏-->
    <uses-permission android:name="android.permission.WAKE_LOCK"/>
    <!--录音权限-->
    <uses-permission android:name="android.permission.RECORD_AUDIO"/>
    <!--修改音频设置-->
    <uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS"/>
    <!--蓝牙权限-->
    <uses-permission android:name="android.permission.BLUETOOTH"/>
    <!--蓝牙连接权限，此权限还需在运行应用时动态申请，否则 Android 12 及以上的设备蓝牙无法正常工作-->
    <uses-permission android:name="android.permission.BLUETOOTH_CONNECT" />
    <!--外置存储卡写入权限-->
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
    <!--蓝牙 startBluetoothSco 会用到此权限-->
    <uses-permission android:name="android.permission.BROADCAST_STICKY"/>
    <!--获取设备信息-->
    <uses-permission android:name="android.permission.READ_PHONE_STATE"/> 

```

权限说明如下表所示。

| 必要性 | 获取的权限 | 使用目的 |
|---|---|---|
|必要权限  | 访问网络权限（INTERNET）|SDK 基本功能都需要在联网的情况下才可以使用|
| ^^ | 网络连接状态（ACCESS_NETWORK_STATE） | 判断网络连接状态及网络是否可用 |
| ^^ | Wifi网络状态（ACCESS_WIFI_STATE和CHANGE_WIFI_STATE） | 判断网络连接状态及网络是否可用 |
| ^^ | 麦克风（RECORD_AUDIO） | 音视频通话时，用于采集声音 |
| ^^ |修改音频设置（MODIFY_AUDIO_SETTINGS）|修改音频设备配置时需要使用该权限|
| 非必要权限 | 设备存储（WRITE_EXTERNAL_STORAGE） | 存储 SDK 配置文件和日志文件 |
| ^^ | 设备存储（READ_EXTERNAL_STORAGE） | 读取 SDK 配置文件和日志文件 |
| ^^ | 蓝牙（BLUETOOTH和BLUETOOTH_CONNECT） | 连接蓝牙耳机和耳麦时需要该权限<note type="notice"><ul><li>BLUETOOTH_CONNECT权限：您需要在代码中动态申请 `android.permission.BLUETOOTH_CONNECT` 权限，否则 Android 12 及以上系统版本的设备会无法使用蓝牙功能，具体信息请参见 [Android 官方说明](https://developer.android.google.cn/about/versions/12/features/bluetooth-permissions?hl=zh-cn)。<li>BLUETOOTH_CONNECT权限：SDK V4.6.56 及之后版本，若用户未赋予该权限，SDK 会通过手机麦克风采集声音。但通话时需要要离手机比较近，对方才能听清声音。<li>BLUETOOTH 权限：仅 Android 6.0 以下版本需要声明，Android 6.0 及以上版本无需声明。</note> |
| ^^ | WAKE_LOCK | 防止通话过程中锁屏 |
| ^^ | 设备信息（READ_PHONE_STATE） | SDK 需要监听电话的打断，在电话呼入时，停止音频的采集，电话结束时，恢复音频采集 |
| ^^ | 更改设备的网络状态（CHANGE_NETWORK_STATE） | SDK 操作多网卡<note type="notice">集成 SDK V5.4.0 及之后版本，需要该权限。</note> |
 
:::
::::::
## 步骤三 导出工程设置
:::::: div custom-tabs
::: tab iOS

为了能够自动化生成完整的 Xcode 工程，需要在`Assets/Editor`目录下创建脚本文件，将 iOS 端的 framework 文件 Embeded 到 Xcode 工程中去，否则，运行 App 的时候，会找不到必要的 framework 而导致崩溃。

```c#
using System.Collections.Generic;
using UnityEditor;
using UnityEditor.Callbacks;
#if UNITY_EDITOR_OSX
using UnityEditor.iOS.Xcode;
using UnityEditor.iOS.Xcode.Extensions;
#endif

public class Builder
{
    [PostProcessBuild(100)]
    public static void OnPostprocessBuild(BuildTarget target, string pathToBuiltProject)
    {
        if(target != BuildTarget.iOS)
        {
            return;
        }

#if UNITY_EDITOR_OSX && !UNITY_2018_3_OR_NEWER
        string projPath = PBXProject.GetPBXProjectPath(pathToBuiltProject);
        var proj = new PBXProject();
        proj.ReadFromString(File.ReadAllText(projPath));

        string targetGuid = proj.GetUnityMainTargetGuid();

        string[] sdkPaths = new string[]{
            "Frameworks/com.netease.game.rtc/Runtime/Plugins/iOS/nertc-c-sdk.framework",
            "Frameworks/com.netease.game.rtc.spatialaudio/Runtime/Plugins/iOS/NERtcAudio3D.framework",
            "Frameworks/com.netease.game.rtc.aihowling/Runtime/Plugins/iOS/NERtcAiHowling.framework",
            "Frameworks/com.netease.game.rtc.ains/Runtime/Plugins/iOS/NERtcAiDenoise.framework",
            "Frameworks/com.netease.game.rtc.nenn/Runtime/Plugins/iOS/NERtcnn.framework",
        };

        foreach (var sdkPath in sdkPaths)
        {
            string sdkGuid = proj.FindFileGuidByRealPath(sdkPath, PBXSourceTree.Source);
            Debug.Log($"sdkGuid:{sdkGuid},sdkPath:{sdkPath}");
            if (!string.IsNullOrEmpty(sdkGuid))
            {
                proj.AddFileToEmbedFrameworks(targetGuid, sdkGuid);
            }
        }

        proj.SetBuildProperty(targetGuid, "LD_RUNPATH_SEARCH_PATHS", "$(inherited) @executable_path/Frameworks");

        //save to project
        File.WriteAllText(projPath, proj.WriteToString());
#endif
    }
}
```
:::
::: tab Android

1. 在 Unity 中打开 **Build Settings** > **Player Settings**，在 **Publishing Settings** 点击 **Keystore Manager** 打开窗口。
2. 选择下拉列表 **Keystor...** > **Create New** > **AnyWhere** 生成新的 keystore 文件。
3. 在 `Assets/Editor` 目录下创建脚本文件，将此 Keystore 文件设置为自动加载。

```
[InitializeOnLoad]
public class PreloadKeystoreSetting
{
#if UNITY_ANDROID
    static PreloadKeystoreSetting()
    {
        PlayerSettings.Android.keystoreName = "user.keystore";
        PlayerSettings.Android.keyaliasName = "nertc";
        PlayerSettings.Android.keystorePass = "000000";
        PlayerSettings.Android.keyaliasPass = "000000";
}
#endif
}
```
</TabItem>
</Tabs>
:::
::::::

## 后续步骤

<a href="https://doc.yunxin.163.com/nertc/quick-start/zY0MjYyMTE" target="_blank">实现音频通话</a>


