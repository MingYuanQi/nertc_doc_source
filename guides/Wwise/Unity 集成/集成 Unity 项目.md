本文介绍如何在 Audiokinetic Wwise 游戏应用设计工具中，为 Unity 工程安装配置网易云信音视频通话 Wwise 插件（简称 NERTC Wwise 插件）。

## 开发环境要求

- Wwise SDK 2022.1.8.8316
- Unity 2020.3.20+
- Android 5+、Android SDK API 21+
- Windows 7+
- iOS 10.0+、Xcode 14+

    ::: note notice
    iOS 移除了对 Bitcode 的支持，如果在 Xcode 14 以下打包应用，需要将 ENABLE_BITCODE 设置为 NO。
    :::

## 第一步：安装插件

1. 前往下载 [NERTC Wwise SDK](https://doc.yunxin.163.com/nertc/resource?platform=wwise)，解压后获取到 NERTC Wwise 插件。

2. 打开 **AudioKinetic Launcher > Plugin-ins** 选项，在右侧页面选择 **Add from archive**，选择下载的 NERTC Wwise 插件包文件进行安装。

3. 等待安装完成后，就可以集成到 Unity 工程。

4. 检查 Unity 工程目录库文件更新状态。如果 Unity 工程目录库文件没有更新，请删除 Unity 项目中所有 Wwise 相关的文件，重新执行步骤 3。

    Wwise 相关的文件：

    - `WwiseUnityIntegration_*.zip`
    - `Assets/Wwise` 目录
    - `Assets/WwiseSettings.xml`

## 第二步：集成 Unity 项目

在 NERTC Wwise 插件包安装完成后，您可以将插件集成到游戏示例或者您的 Unity 项目中。

在集成 NERTC Wwise 之前，需要确保已经安装好插件包。

1. 打开 Audiokinetic Launcher 应用，选择左侧 Unity 标签页后，可以看到想要集成的 Unity 项目。选择该工程，并单击 **Intergate Wwise into Project** 按钮集成到到此 Unity 项目。

2. 集成完成后，Audiokinetic Launcher 会自动将所需的插件拷贝到 **{YOUR_UNITY_PROJECT}/Assets/wwise/Development/Plugins** 目录下相应的 DSP 文件夹下。自动集成完成后，需要手动将 **{Wwise-INSTALL-ROOT}/SDK/plugins/nertc/Unity/GameVoiceWwiseDemo/Assets/GameSDK** 文件夹拷贝到 **{YOUR_UNITY_PROJECT}/Assets** 目录。

3. 插件准备好之后，需要在 **Assets/Editor/Build.cs** 中将 **nertc-c-sdk** 相关的插件导入到工程中，才能被打包出去。参考以下示例。

    示例代码

    ```Java
    using System.Collections.Generic;
    using System.IO;
    using UnityEditor;
    using UnityEditor.Callbacks;
    using UnityEngine;
    #if UNITY_EDITOR_OSX
    using UnityEditor.iOS.Xcode;
    using UnityEditor.iOS.Xcode.Extensions;
    #endif

    public class Builder : Editor, UnityEditor.Build.IPostprocessBuild, UnityEditor.Build.IPreprocessBuild
    {
        public int callbackOrder
        {
            get { return 1; }
        }

        public void OnPreprocessBuild(BuildTarget target, string path)
        {
            if (target != BuildTarget.StandaloneWindows &&
                target != BuildTarget.StandaloneWindows64 &&
                target != BuildTarget.iOS &&
                target != BuildTarget.StandaloneOSX &&
                target != BuildTarget.Android)
            {
                return;
            }

            var importers = UnityEditor.PluginImporter.GetAllImporters();

            foreach (var pluginImporter in importers)
            {
                if ((target == BuildTarget.iOS && pluginImporter.assetPath.Contains("iOS"))
                    && pluginImporter.assetPath.Contains("libWwiseRtcSdkPlugin.a"))
                {
                    UnityEngine.Debug.Log("OnPreprocessBuild import libWwiseRtcSdkPlugin.a for " + target + " at " + pluginImporter.assetPath);
                    pluginImporter.SetCompatibleWithPlatform(target, true);
                    continue;
                }
                if (!pluginImporter.assetPath.Contains("nertc-c-sdk"))
                {
                    continue;
                }
                if ((target != BuildTarget.iOS || !pluginImporter.assetPath.Contains("iOS"))
                && ((target != BuildTarget.StandaloneWindows && target != BuildTarget.StandaloneWindows64) || !pluginImporter.assetPath.Contains("Windows"))
                && (target != BuildTarget.StandaloneOSX || !pluginImporter.assetPath.Contains("Mac"))
                && (target != BuildTarget.Android || !pluginImporter.assetPath.Contains("Android")))
                {
                    continue;
                }
                UnityEngine.Debug.Log("OnPreprocessBuild import library (target " + target + ") at " + pluginImporter.assetPath);
                pluginImporter.SetCompatibleWithPlatform(target, true);
            }

        }
        public void OnPostprocessBuild(BuildTarget target, string path)
        {
            if (target != BuildTarget.StandaloneWindows &&
                target != BuildTarget.StandaloneWindows64 &&
                target != BuildTarget.iOS &&
                target != BuildTarget.StandaloneOSX &&
                target != BuildTarget.Android)
            {
                return;
            }

            var importers = UnityEditor.PluginImporter.GetAllImporters();

            foreach (var pluginImporter in importers)
            {
                if ((target == BuildTarget.iOS && pluginImporter.assetPath.Contains("iOS"))
                    && pluginImporter.assetPath.Contains("libWwiseRtcSdkPlugin.a"))
                {
                    UnityEngine.Debug.Log("OnPostprocessBuild import libWwiseRtcSdkPlugin.a for " + target + " at " + pluginImporter.assetPath);
                    pluginImporter.SetCompatibleWithPlatform(target, false);
                    UnityEditor.AssetDatabase.ImportAsset(pluginImporter.assetPath);
                    continue;
                }
                if (!pluginImporter.assetPath.Contains("nertc-c-sdk"))
                {
                    continue;
                }
                if ((target != BuildTarget.iOS || !pluginImporter.assetPath.Contains("iOS"))
                && ((target != BuildTarget.StandaloneWindows && target != BuildTarget.StandaloneWindows64) || !pluginImporter.assetPath.Contains("Windows"))
                && (target != BuildTarget.StandaloneOSX || !pluginImporter.assetPath.Contains("Mac"))
                && (target != BuildTarget.Android || !pluginImporter.assetPath.Contains("Android")))
                {
                    continue;
                }
                UnityEngine.Debug.Log("OnPostprocessBuild import library (target " + target + ") at " + pluginImporter.assetPath);
                pluginImporter.SetCompatibleWithPlatform(target, false);
                UnityEditor.AssetDatabase.ImportAsset(pluginImporter.assetPath);
            }
        }
    }
    ```

4. 增加音频权限设置。

    :::::: div custom-tabs
    ::: tab Android
    需要在 `Assets/Plugin/Android/AndroidManifest.xml` 相应的权限，部分权限必须要动态申请权限才能使用，例如蓝牙连接权限，音频权限等。

    ```Java
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

    :::
    ::: tab iOS

    需要在 Unity 工程的 `Player Settings` 中增加 `Microphone Usage Description` 的描述文案。如果没有，会导致 App 运行时崩溃.

    :::
    ::::::

## 下一步

完成上述配置后，您可以参考 [在 Wwise 中的安装和使用](https://doc.yunxin.163.com/nertc/guide/TMwMzI3NjE?platform=wwise) 创建 Wwise Event 等来实现相应的功能。