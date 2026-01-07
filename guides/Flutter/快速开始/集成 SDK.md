本文为您介绍了 Flutter 端集成 NERTC SDK 的操作步骤，帮助您快速集成 SDK 并实现实时音视频通话的基本功能。

## 开发环境要求

<style>
table th:first-of-type {width: 10%;}
table th:nth-of-type(2) {width: 23%;}
table th:nth-of-type(3) {width: 23%;}
table th:nth-of-type(4) {width: 23%;}
table th:nth-of-type(5) {width: 23%;}
</style>

在开始运行工程之前，请您准备以下开发环境：

- [Flutter](https://docs.flutter.dev/development/tools/sdk/releases?tab=windows#macos) 3.10.0 及以上版本。
- Dart 3.0.0 及以上版本。
- [安装 Flutter 和 Dart 插件](https://docs.flutter.dev/get-started/editor?)
- 开发工具：
    平台 | 开发工具 | 设备要求 | 其他要求 | 开发语言 |
    ---- | ------- | ------- | ------- | ------- |
    **Android** | [Android Studio](https://developer.android.com/studio/releases?hl=zh-cn) 4.1+ | Android 5.0+ | - | Java |
    **iOS** | Xcode 11+ | iOS 11+ | 有效的开发者签名 | Objective-C |
    **macOS** | Xcode 11+ | macOS X 11.5+ | 有效的开发者签名 | - |
    **Windows** | Microsoft Visual Studio 2017+ | Windows 7+ | Microsoft Visual C++ 2017+ | - |

## 第一步：（可选）创建项目

您可以参考此步骤创建新项目，若是需要集成到已有的项目中，请忽略该步骤。创建 Flutter 项目的详细操作步骤请参考 [Flutter get-started](https://docs.flutter.dev/get-started/test-drive?tab=androidstudio)。

:::::: div linked-codes
::: code Android Studio

1. 在 Android Studio 的顶部菜单依次选择 **File** > **New** > **New Flutter Project** 新建工程。选择 **Flutter Application**，单击 **Next**。

    <img alt="Flutter_Application.png" src="https://yx-web-nosdn.netease.im/common/7fd6e6c3f3725827bb0fe2900342e651/Flutter_Application.png" style="width:60%;border: 1px solid #BFBFBF;border-radius:0px;">

2. 配置 Project 信息。

    设置项目名称、项目位置和 Flutter SDK 位置路径。
    ::: note note
    - **Flutter SDK path** 请选择 Flutter SDK 根目录。
    - 项目名称和路径请设置为小写英文字母，且用下划线分割开。
    :::

    <img alt="Project 配置 Flutter_.png" src="https://yx-web-nosdn.netease.im/common/c6db6c5a25554dc3364fbea930aac093/Project配置Flutter_.png" style="width:60%;border: 1px solid #BFBFBF;border-radius:0px;">

3. 设置包名，单击 **Finish**。

    ::: note note
    如果您的应用计划上架商店，建议一开始的时候把这个全网唯一的包名设置好，因为应用上架之后就不能再修改了。
    :::

:::
::: code Xcode

1. 打开 Xcode，从菜单栏中选择 **File** > **New** > **Project**。

2. 在弹出的 **Choose a template for your new project** 窗口中，选择 **Flutter**，单击 **Next**。

3. 填写项目名称、组织名称和组织标识符，选择项目存储位置，并选择所需的语言和界面，单击 **Next**。
    ::: note note

    项目名称和路径请设置为小写英文字母，且用下划线分割开。
    :::

4. 选择 Flutter SDK 的路径，并选择所需的项目文件夹。

:::
::::::

## 第二步：导入 SDK

NERTC Flutter SDK 已正式发布到 [pub.dev](https://pub.dev/packages/nertc_core)，您可以在 pub 库中查询最新版本，通过配置 `pubspec.yaml` 自动下载更新。

在项目的 `pubspec.yaml` 文件中添加以下依赖项：

```
environment:
  sdk: ">=3.0.0 <4.0.0"
  flutter: ">=3.10.0"

# 依赖项
dependencies:
  flutter:
    sdk: flutter
  # NERTC Flutter SDK 依赖项
  nertc_core: ^5.8.23
  cupertino_icons: ^1.0.3
  # 权限处理插件依赖项
  permission_handler: ^12.0.1
```

::: note note
示例代码中的版本号为举例，请替换为最新的版本号。您可以在 [pub.dev](https://pub.dev/packages/nertc_core) 网站上查询 `nertc_core` 的最新版本号。

<!-- - 从 4.6.52 版本开始，SDK 包名从 `nertc` 改为 `nertc_core`。如果您之前使用的是旧版本，请注意更新包名。 -->
:::

## 第三步：添加权限

通过 Flutter SDK 实现音视频通话之前，需要开通摄像头和麦克风的使用权限，以开启视频和语音通话功能。

:::::: div linked-codes
::: code Android 端

打开 `app/src/main/AndroidManifest.xml` 文件，添加必要的设备权限。例如：

```XML
//网络相关
 <uses-permission android:name="android.permission.INTERNET"/>
 <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
 <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
 <uses-permission android:name="android.permission.CHANGE_WIFI_STATE"/>
//防止通话过程中锁屏
 <uses-permission android:name="android.permission.WAKE_LOCK"/>
//视频权限
 <uses-permission android:name="android.permission.CAMERA"/>
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
//允许应用程序使用 camera 硬件资源
 <uses-feature android:name="android.hardware.camera"/>
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
| ^^ | 摄像头（CAMERA 和 camera.autofocus） | 视频通话时，用于采集视频画面 |
| ^^ | 麦克风（RECORD_AUDIO） | 音视频通话时，用于采集声音 |
| ^^ | 修改音频设置（MODIFY_AUDIO_SETTINGS） | 修改音频设备配置时需要使用该权限 |
| 非必要权限 | 设备存储（WRITE_EXTERNAL_STORAGE） | 存储 SDK 配置文件和日志文件，推荐添加以便于问题排查 |
| ^^ | 设备存储（READ_EXTERNAL_STORAGE） | 读取 SDK 配置文件和日志文件，推荐添加以便于问题排查 |
| ^^ | 蓝牙（BLUETOOTH） | 适用于所有 Android 版本，仅 Android 6.0 以下版本需要声明，Android 6.0 及以上版本无需声明 |
| ^^ | 蓝牙连接（BLUETOOTH_CONNECT） | 适用于 Android 12 及以上版本，需在代码中动态申请 `android.permission.BLUETOOTH_CONNECT` 权限，否则无法使用蓝牙功能 |
| ^^ | 锁屏控制（WAKE_LOCK） | 防止通话过程中锁屏，确保通话流畅不中断 |
| ^^ | 设备信息（READ_PHONE_STATE） | SDK 需要监听电话的打断，在电话呼入时暂停音频采集，电话结束时恢复音频采集 |
| ^^ | 更改设备的网络状态（CHANGE_NETWORK_STATE） | SDK 5.4.0 及之后版本需要，用于操作多网卡场景 |

::: note notice
关于 Android 权限申请的重要说明：

1. **动态权限申请**：Android 6.0 及以上版本要求部分重要权限必须申请动态权限，不能只通过 `AndroidMainfest.xml` 文件申请静态权限。

2. **蓝牙权限特别说明**：
   - BLUETOOTH_CONNECT 权限必须在 Android 12 及以上设备上动态申请，否则蓝牙功能将无法正常工作
   - 若用户未赋予 BLUETOOTH_CONNECT 权限，SDK 5.4.0 及之后版本会自动降级使用手机麦克风采集声音，但通话时需要靠近手机才能保证声音清晰

3. **权限申请示例代码**：
   以下是使用 `permission_handler` 插件动态申请权限的示例代码

```Dart
import 'package:permission_handler/permission_handler.dart';

// 在适当的时机（如页面加载或按钮点击时）请求权限
Future<bool> requestPermissions() async {
  // 定义需要的权限列表
  final permissions = [Permission.camera, Permission.microphone];
  
  // 根据平台添加额外权限
  if (Platform.isAndroid) {
    permissions.add(Permission.storage);
    
    // 如果 Android 版本 >= 12，添加蓝牙连接权限
    if (int.parse(await _getAndroidVersion()) >= 31) {
      permissions.add(Permission.bluetoothConnect);
    }
  }
  
  // 检查哪些权限尚未获得
  List<Permission> notGrantedPermissions = [];
  for (var permission in permissions) {
    PermissionStatus status = await permission.status;
    if (status != PermissionStatus.granted) {
      notGrantedPermissions.add(permission);
    }
  }
  
  // 如果所有权限都已获得，直接返回成功
  if (notGrantedPermissions.isEmpty) {
    return true;
  }
  
  // 请求未获得的权限
  Map<Permission, PermissionStatus> statuses = await notGrantedPermissions.request();
  
  // 检查是否所有权限都已获得
  bool allGranted = true;
  statuses.forEach((permission, status) {
    if (status != PermissionStatus.granted) {
      allGranted = false;
    }
  });
  
  return allGranted;
}

// 获取 Android 版本号
Future<String> _getAndroidVersion() async {
  if (!Platform.isAndroid) return '0';
  try {
    // 这里可以使用 device_info_plus 插件获取 Android 版本
    // 简化示例，实际使用时请替换为正确的实现
    return '31'; // 默认返回 Android 12 版本号
  } catch (e) {
    return '0';
  }
}
```

如果权限被拒绝，您可能需要引导用户前往设置页面手动开启权限：

```Dart
void _openAppSettings() {
  showDialog(
    context: context,
    builder: (BuildContext context) => AlertDialog(
      title: Text('需要权限'),
      content: Text('为了正常使用音视频通话功能，请前往设置页面手动开启相关权限。'),
      actions: <Widget>[
        TextButton(
          child: Text('取消'),
          onPressed: () => Navigator.of(context).pop(),
        ),
        TextButton(
          child: Text('去设置'),
          onPressed: () {
            Navigator.of(context).pop();
            openAppSettings();
          },
        ),
      ],
    ),
  );
}
```
:::

:::
::: code iOS 端
请给 App 授权麦克风、摄像头和 Wi-Fi 的使用权限。

编辑 `info.plist` 文件，添加以下三项。

- **Privacy - Microphone Usage Description**，并填入麦克风使用目的提示语，例如："音视频通话功能需要使用您的麦克风"。
- **Privacy - Camera Usage Description**，并填入摄像头使用目的提示语，例如："视频通话功能需要使用您的摄像头"。
- **Application uses Wi-Fi**，设置为 **YES**。

<img alt="Xnip2022-12-02_16-46-19.jpg" src="https://yx-web-nosdn.netease.im/common/6bb4a91a0cb760c799ac2921ae41b942/Xnip2022-12-02_16-46-19.jpg" style="width:60%;border: 1px solid #BFBFBF;border-radius:0px;">

::: note note
如果使用 Platform View，并且 Flutter 版本低于 1.22，请在 **Info** > **Custom iOS Target Properties** 中，增加字段 `io.flutter.embedded_views_preview`，并设置 Value 为 YES。此配置对于 Flutter 1.22 及以上版本不再需要。
:::

:::
::: code macOS 端
请给 App 授权麦克风、摄像头的使用权限。

编辑 `info.plist` 文件，添加以下项：

- **Privacy - Microphone Usage Description**，并填入麦克风使用目的提示语，例如："音视频通话功能需要使用您的麦克风"。
- **Privacy - Camera Usage Description**，并填入摄像头使用目的提示语，例如："视频通话功能需要使用您的摄像头"。

此外，还需要在 `macOS/Runner/DebugProfile.entitlements` 和 `macOS/Runner/Release.entitlements` 中添加以下权限：

```xml
<key>com.apple.security.device.audio-input</key>
<true/>
<key>com.apple.security.device.camera</key>
<true/>
```
:::
::: code Windows 端
Windows 平台通常会在运行时自动请求麦克风和摄像头权限，无需额外配置。但您需要确保设备上的隐私设置允许应用程序访问麦克风和摄像头。

您可以通过以下步骤检查 Windows 设备的隐私设置：

1. 打开 Windows 设置 > 隐私 > 摄像头。
2. 确保 **允许应用访问你的摄像头** 选项已启用。
3. 打开 Windows 设置 > 隐私 > 麦克风。
4. 确保 **允许应用访问你的麦克风** 选项已启用。
:::
::::::

## 第四步：配置防代码混淆（仅 Android 端）

代码混淆是指使用简短无意义的名称重命名类、方法、属性等，增加逆向工程的难度，保障 Android 程序源码的安全性。为了避免因重命名类，导致调用 NERTC SDK 异常，您需要配置防代码混淆。

在 `/example/android/app` 目录下的 `proguard-rules.pro` 文件中，为 NERTC SDK 添加 -keep 类的配置，可以防止混淆 NERTC SDK 公共类名称。

```
-keep class com.netease.lava.** {*;}
-keep class com.netease.yunxin.** {*;}
```

## 平台特定配置

### iOS 和 macOS

如果您的应用需要支持后台音频，请在 `info.plist` 中添加以下配置：

```xml
<key>UIBackgroundModes</key>
<array>
    <string>audio</string>
</array>
```

### Android

如需支持后台运行，请在 `AndroidManifest.xml` 文件的 `<application>` 标签中添加：

```xml
<service
    android:name="com.netease.lava.nertc.sdk.service.NERtcForegroundService"
    android:foregroundServiceType="mediaProjection" />
```

## <span id="后续步骤">下一步</span>

[实现音视频通话](https://doc.yunxin.163.com/nertc/guide/jMwNzY0NjM?platform=flutter)。