<!--keywords: 语音通话,音频通话,webrtc,RTC,集成 SDK -->
<!--和互动直播对应文档基本相同，替换产品名称后即可重用-->

本文为您介绍了 Android 端集成 SDK 的操作步骤，帮助您快速集成 SDK 并实现音频通话的基本功能。

## <span id="前提条件">前提条件</span>

在开始运行工程之前，请您准备以下开发环境：

- Android SDK API 等级 19 或以上。
- Android Studio 3.0 或以上版本。
- Android 系统 4.4 或以上版本的移动设备。
- [Gradle](https://services.gradle.org/distributions/) 6.7.1 以及以上版本。

## <span id="SDK 目录">SDK 目录</span>

| 文件/文件夹名称 | 是否必选 | 说明 |
| --- | --- | --- |
| nertc-sdk-5.x.xx.jar | 是 | 音频库。 |
| libnertc_sdk.so | 是 | ^^ |
| part-nertc-sdk-part-5.x.xx.jar | 是 | G1 兼容库。 |
| part-GrowDevice-1.7.2.4.jar | 是 | 智企库。 |
| libgrowease.so | 是 | 智企库。 |
| libNERtcnn.so | 否 | 神经网络插件。<note type="notice">集成 AI 啸叫检测时，才需集成 `libNERtcnn.so` 插件。<br>基础音频不需要集成该库文件。</note> |
| libNERtcAiHowling.so | 否 | AI 啸叫检测插件。 |
| libNERtcAiDenoise.so | 否 | AI 降噪插件。 |
| libNERtcAudio3D.so | 否 | 空间音效插件。（自 5.4.0 版本开始提供） |

## <span id="集成 SDK">集成 SDK</span>

### <span id="Maven 集成">Maven 集成（推荐）</span>

1. （可选）创建新项目。
    ::: details 您可以参考此步骤创建新项目，若是需要集成到已有的项目中，请忽略该步骤。
    1. 在 Android Studio 里，在顶部菜单依次选择 **File** > **New** > **New Project** 新建工程，再依次选择 **Phone and Tablet** > **Empty Activity**，单击 **Next**。
    <img style="width:50%" src="https://yx-web-nosdn.netease.im/common/5c330c3371274f9cb714a48f8865d3bc/android_create_project.png" alt="image" />
    2. <a href="https://developer.android.com/studio/projects/create-project" target="_blank">创建 Android 项目</a> 成功后，Android Studio 会自动开始同步 gradle, 您需要等同步成功后再进行下一步操作。
    :::

2. 配置项目的 Gradle 文件。

    进入项目根目录，打开 `app/build.gradle` 文件，在 `allprojects` 中加入以下代码。

    ```Groovy
    allprojects {
        repositories {
            mavenCentral()
            maven { url 'https://www.jitpack.io' }
            google()

        }
    }
    ```

    在 `app/build.gradle` 文件中设置 Java 编译版本。

    ```Groovy
    android {
            compileOptions {
            // SDK 依赖的 JDK 版本为 Java 8
            sourceCompatibility JavaVersion.VERSION_1_8
            targetCompatibility JavaVersion.VERSION_1_8
            }

        }

    ```

3. 引入 NERTC Android SDK。

    请在项目对应模块的 `build.gradle` 中加入以下代码。

    ```Groovy
    //集成 SDK 但不使用任何插件
    implementation 'com.netease.yunxin:nertc-audio:5.6.50'

    //集成 SDK 且使用部分插件，以 AI 啸叫检测为例
    implementation 'com.netease.yunxin:nertc-audio:5.6.50'
    implementation 'com.netease.yunxin:nertc-nenn:5.6.50'
    implementation 'com.netease.yunxin:nertc-aihowling:5.6.50'
    ```

    其中，`5.6.50` 为待集成的 NERTC SDK 版本号举例。建议使用最新版本，您可以在 <a href="https://doc.yunxin.163.com/nertc/sdk-download" target="_blank">网易云信 SDK 下载中心</a> 查看 NERTC SDK 最新版本的版本号。若要使用其他版本，请联系网易云信技术支持获取对应的版本号。

    `artifactId` 中请填入待引入的动态库的名称，具体说明如下表所示。

    功能/插件 | `artifactId` 的值 | 集成的动态库 |
    ---- | ---- | ----
    音频 | `com.netease.yunxin:nertc-audio` | 必选基础库（`libnertc_sdk.so`、`libgrowease.so` 等） |
    神经网络 | `com.netease.yunxin:nertc-nenn` | 神经网络库: `libNERtcnn.so` <note type="notice"> 只有集成音频啸叫检测时，才需要集成 `libNERtcnn.so` 库文件。基础音频不需要集成该库文件。</note> |
    AI 降噪 | `com.netease.yunxin:nertc-aidenoise` | AI 降噪库：`libNERtcAiDenoise.so`
    AI 啸叫检测 | `com.netease.yunxin:nertc-aihowling` | AI 啸叫检测库：`libNERtcAiHowling.so`
    空间音效 | `com.netease.yunxin:nertc-audio3d` | 空间音效库：`libNERtcAudio3D.so`

5. 指定 App 使用的 CPU 架构

    请在项目对应模块的 `build.gradle` 的 `defaultConfig` 中加入以下代码。

    ```Groovy
    defaultConfig {
        ndk {
                abiFilters "armeabi-v7a", "arm64-v8a", "x86",
        }
    }
    ```
    ::: note note
    - 目前 NERTC Android SDK 支持 armeabi-v7a、arm64-v8a 和 x86 架构。
    - 建议根据实际情况决定要支持的架构。通常在发布 App 时只需要保留 "armeabi-v7a" 和 "arm64-v8a" 即可，可以减少 APK 包大小。
    :::

### <span id="手动集成">手动集成</span>

1. （可选）创建新项目。
    ::: details 您可以参考此步骤创建新项目，若是需要集成到已有的项目中，请忽略该步骤。
    1. 在 Android Studio 里，在顶部菜单依次选择 **File** > **New** > **New Project** 新建工程，再依次选择 **Phone and Tablet** > **Empty Activity**，单击 **Next**。
    <img style="width:50%" src="https://yx-web-nosdn.netease.im/common/5c330c3371274f9cb714a48f8865d3bc/android_create_project.png" alt="image" />
    2. <a href="https://developer.android.com/studio/projects/create-project" target="_blank">创建 Android 项目</a> 成功后，Android Studio 会自动开始同步 gradle, 您需要等同步成功后再进行下一步操作。
    :::

2. 前往 <a href="https://yunxin.163.com/im-sdk-demo?category=nrtc" target="_blank">SDK 下载页面</a> 获取当前最新版本 SDK，或联系网易云信技术支持获取对应版本的 SDK。
3. 解压后将对应的文件拷贝至项目路径中。
    <table border="1">
    <tr>
      <th width="30%"><b>文件/文件夹</b></th>
      <th width="50%"><b>项目路径</b></th>
    </tr>
    <tr>
      <td>nertc-sdk-x.x.x.jar</td>
      <td>/app/libs/</td>
    </tr>
    <tr>
      <td>nertc-sdk-part-5.x.xx.jar、GrowDevice-1.7.2.4.jar</td>
      <td>/app/libs/</td>
    </tr>
      <tr>
      <td>arm64-v8a<br/>arm64-v7a<br/>x86</td>
      <td>/app/src/main/jniLibs/</td>
    </tr>
    </table>

    <img alt="add_jar.png" src="https://yx-web-nosdn.netease.im/common/a2eecfd52922cb575d38f2539fd850a0/add_jar.png" style="width:60%;border: 1px solid #BFBFBF;">

    ::: note note
    - 若无对应文件夹，您需要在对应路径下新建文件夹。
    - 无特殊情况，可忽略 `part` 文件夹。
    - `armeabi-v7a`、`arm64-v8a` 和 `x86` 目录下的动态库包括 AI 啸叫检测等可选库，请按需拷贝到对应的目录，具体请参考下表。
    :::

    功能/插件 | 集成的动态库 |
    ---- | ----
    音频 | 必选基础库：`libnertc_sdk.so`、、`libgrowease.so`、`libgrowease.so`、`part-nertc-sdk-part-x.x.x.jar`、`part-GrowDevice-x.x.x.jar` |
    AI 降噪 | AI 降噪库：`libNERtcAiDenoise.so`
    AI 啸叫检测 | <ul><li>AI 啸叫检测库：`libNERtcAiHowling.so`<li>神经网络库: `libNERtcnn.so`
    空间音效 | 空间音效库：`libNERtcAudio3D.so`

4. 在 **app/build.gradle** 文件中设置 **libs** 路径。

    ```Groovy
    android {
          ...
        compileOptions {
            // SDK 依赖的 JDK 版本为 Java 8
            sourceCompatibility JavaVersion.VERSION_1_8
            targetCompatibility JavaVersion.VERSION_1_8
        }
        ...

        dependencies {
            implementation fileTree(dir: "libs", include: ["*.jar"])
            ...
        }
    }
    ```

5. 指定 App 使用的 CPU 架构。

    请在项目对应模块的 `build.gradle` 的 `defaultConfig` 中加入以下代码。

    ```Groovy
    defaultConfig {
        ndk {
                abiFilters "armeabi-v7a", "arm64-v8a", "x86",
        }
    }
    ```
    ::: note note
    - 目前 NERTC Android SDK 支持 armeabi-v7a、arm64-v8a 和 x86 架构。
    - 建议根据实际情况决定要支持的架构。通常在发布 App 时只需要保留 "armeabi-v7a" 和 "arm64-v8a" 即可，可以减少 APK 包大小。
    :::

6. 单击 <b>File > Sync Project With Gradle Files</b> 按钮，直到同步完成。

## <span id="添加权限">添加权限</span>

打开 `app/src/main/AndroidManifest.xml` 文件，添加必要的设备权限。

例如：

```XML
//网络相关
 <uses-permission android:name="android.permission.INTERNET"/>
 <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
 <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
 <uses-permission android:name="android.permission.CHANGE_WIFI_STATE"/>
//防止通话过程中锁屏
 <uses-permission android:name="android.permission.WAKE_LOCK"/>

//录音权限
 <uses-permission android:name="android.permission.RECORD_AUDIO"/>
//修改音频设置
 <uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS"/>
//蓝牙权限
 <uses-permission android:name="android.permission.BLUETOOTH"/>
//蓝牙连接权限，此权限还需在运行应用时动态申请，否则 Android 12 及以上的设备蓝牙无法正常工作
 <uses-permission android:name="android.permission.BLUETOOTH_CONNECT" />
//外置存储卡写入权限
 <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
//蓝牙 startBluetoothSco 会用到此权限
 <uses-permission android:name="android.permission.BROADCAST_STICKY"/>
// 5.4.0 及之后版本需要，更改设备的网络状态，如打开或关闭网络连接、更改网络类型等。
 <uses-permission android:name="android.permission.CHANGE_NETWORK_STATE"/>
 //获取设备信息
 <uses-permission android:name="android.permission.READ_PHONE_STATE"/>
 //自动对焦
 <uses-feature android:name="android.hardware.camera.autofocus"/>
 //如果 Android targetSdkVersion 大于等于 31，需要添加以下标签，否则虚拟背景功能无法使用
 <application>
 <uses-native-library android:name="libOpenCL.so" android:required="false"/>
 </application>
 ......//App 需要的其他设备权限
 ```

权限说明如下表所示。

| 必要性 | 获取的权限 | 使用目的 |
| --- | --- | --- |
| 必要权限 | 访问网络权限（INTERNET） | SDK 基本功能都需要在联网的情况下才可以使用 |
| ^^ | 网络连接状态（ACCESS_NETWORK_STATE） | 判断网络连接状态及网络是否可用 |
| ^^ | Wifi 网络状态（ACCESS_WIFI_STATE 和 CHANGE_WIFI_STATE） | 判断网络连接状态及网络是否可用 |
| ^^ | 麦克风（RECORD_AUDIO） | 音视频通话时，用于采集声音 |
| ^^ | 修改音频设置（MODIFY_AUDIO_SETTINGS） | 修改音频设备配置时需要使用该权限 |
| 非必要权限 | 设备存储（WRITE_EXTERNAL_STORAGE） | 存储 SDK 配置文件和日志文件 |
| ^^ | 设备存储（READ_EXTERNAL_STORAGE） | 读取 SDK 配置文件和日志文件 |
| ^^ | 蓝牙（BLUETOOTH 和 BLUETOOTH_CONNECT） | 连接蓝牙耳机和耳麦时需要该权限<note type="notice"><ul><li>BLUETOOTH_CONNECT 权限：您需要在代码中动态申请 `android.permission.BLUETOOTH_CONNECT` 权限，否则 Android 12 及以上系统版本的设备会无法使用蓝牙功能，具体信息请参考 [Android 官方说明](https://developer.android.google.cn/about/versions/12/features/bluetooth-permissions?hl=zh-cn)。<li>BLUETOOTH_CONNECT 权限：SDK 4.6.56 及之后版本，若用户未赋予该权限，SDK 会通过手机麦克风采集声音。但通话时需要要离手机比较近，对方才能听清声音。<li>BLUETOOTH 权限：仅 Android 6.0 以下版本需要声明，Android 6.0 及以上版本无需声明。</note> |
| ^^ | WAKE_LOCK | 防止通话过程中锁屏 |
| ^^ | 设备信息（READ_PHONE_STATE） | SDK 需要监听电话的打断，在电话呼入时，停止音频的采集，电话结束时，恢复音频采集 |
| ^^ | 更改设备的网络状态（CHANGE_NETWORK_STATE） | SDK 操作多网卡<note type="notice">集成 SDK 5.4.0 及之后版本，需要该权限。</note> |

## <span id="防止代码混淆">配置防代码混淆</span>

代码混淆是指使用简短无意义的名称重命名类、方法、属性等，增加逆向工程的难度，保障 Android 程序源码的安全性。为了避免因重命名类，导致调用 NERTC SDK 异常，您需要配置防代码混淆。

在 `proguard-rules.pro` 文件中，为 NERTC SDK 添加 `-keep` 类的配置，防止混淆 NERTC SDK 公共类名称。

```Groovy
-keep class com.netease.lava.** {*;}
-keep class com.netease.yunxin.** {*;}
```

## <span id="后续步骤">后续步骤</span>

<a href="https://doc.yunxin.163.com/nertc/quick-start/DQ4MDk2NTA" target="_blank">实现音视频通话</a>

## <span id="常见问题">常见问题</span>

- [如何减小集成 SDK 后的 App 包体积？](https://doc.yunxin.163.com/nertc/quick-start/Tk3NDYxNzQ?platform=android)

- 集成编译过程中出现 `<uses-native-library>` 标签找不到

   **问题现象**

    使用 Maven 下载的包进行集成 5.3.0 及以上版本的 SDK 时，出现编译错误，提示：`AAPT: error: unexpected element <uses-native-library> found in <manifest><application>`。

   **问题解决**

    将 [Gradle](https://services.gradle.org/distributions/) 版本升级到 6.7.1 或更高版本。由于旧版 Gradle 不支持 `<uses-native-library>` 标签，升级到新版本后即可解决该问题。