您可以通过跑通 Sample Code，体验网易云信一对一音视频通话功能。

## <span id="前提条件">前提条件</span>
请确认您已完成以下操作：

- [创建应用并获取 App Key](https://doc.yunxin.163.com/console/guide/TIzMDE4NTA?platform=console)。
- [开通音视频通话 2.0 服务](https://doc.yunxin.163.com/nertc/quick-start/TEzNzQwNzM)。

## <span id="前提条件">开发环境要求</span>

在开始运行工程之前，请您准备以下开发环境：

- [Flutter](https://docs.flutter.dev/development/tools/sdk/releases?tab=windows#macos) 3.0.0 及以上版本。
- Dart 2.17.0 及以上版本。
- Android 端开发：
  - [Android Studio](https://developer.android.com/studio/releases?hl=zh-cn) 4.1 及以上版本。
  - App 要求 Android 5.0 及以上版本 Android 设备。

- iOS 端开发：
  - Xcode 11.0 及以上版本。
  - App 要求 iOS 11 及以上版本 iOS 设备。
  - 请确保您的项目已设置有效的开发者签名。

- [安装 Flutter 和 Dart 插件](https://docs.flutter.dev/get-started/editor?)

## <span id="快速跑通 Sample Code">快速跑通 Sample Code</span>

::: note note
在运行示例项目之前，请在云信控制台中为指定应用[开通调试模式](https://doc.yunxin.163.com/nertc/quick-start/TQ0MTI2ODQ?platform=android#修改鉴权方式)。调试模式建议只在集成开发阶段使用，请在应用正式上线前改回安全模式。
:::

1. 下载[语音通话和视频通话的示例项目源码](https://github.com/netease-im/G2-API-Examples/tree/main/flutter/nertc_api_example)仓库至您本地工程。
2. 在 `nertc_api_example/lib/config.dart` 文件中配置应用的 AppKey。

    ```
    class Config {
    //替换为您自己的appkey
    static const String APP_KEY = 'Your AppKey';
    }
    ```
3. 在工程根目录执行如下命令引入依赖。
    ```
    flutter pub get
    ```
4. 编译运行。

    :::::: div custom-tabs
    ::: tab iOS
    1. 打开终端，在 `Podfile` 所在文件夹中执行如下命令进行安装：
        ```
        pod install
        ``` 
    2. 完成安装后，通过 Xcode 打开 `Runner.xcworkspace` 工程。

    3. 编译并运行 Demo 工程。

    :::

    ::: tab Android
 
    i.在工程根目录执行如下命令：
      ```
      flutter run
      ```
    
    ii.使用 Android Studio（4.1及以上的版本）打开源码工程，单击运行即可。


    :::

    ::::::


5. 选中设备直接运行，即可体验 Demo。
::: note note
建议在真机上运行，不支持模拟器调试。
:::




## 目录结构

```
lib
   ├── AudioCall  //语音通话
   │   ├── audioCallingPage.dart
   │   └── callingPage.dart
   ├── VideoCall  //视频通话
   │   ├── videoCallingPage.dart
   │   └── videoViewPage.dart
   ├── config.dart //配置appkey
   └── main.dart //首页
```

