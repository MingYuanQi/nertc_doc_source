<!--keywords: 音视频通话,RTC,跑通,示例项目,Sample Code -->
您可以通过跑通 Sample Code，体验网易云信音视频通话功能，包括一对一音视频通话和多人音视频通话。

## <span id="前提条件">前提条件</span>
请确认您已完成以下操作：

- [创建应用并获取 App Key](https://doc.yunxin.163.com/console/guide/TIzMDE4NTA?platform=console)。
- [开通音视频通话服务](https://doc.yunxin.163.com/nertc/quick-start/zg3MTgwOTc)。



## <span id="开发环境要求">开发环境要求</span>

在开始运行工程之前，请您准备以下开发环境：

- Xcode 11 及以上版本。
- mac OS X 10.11 或以上版本的 Mac 设备。
- 项目已配置有效的开发者签名。

## <span id="快速跑通 Sample Code">快速跑通 Sample Code</span>

::: note note
在运行示例项目之前，请在云信控制台中为指定应用[开通调试模式](https://doc.yunxin.163.com/nertc/quick-start/TAzMDY2MTQ?platformId=50251#修改鉴权方式)。调试模式建议只在集成开发阶段使用，请在应用正式上线前改回安全模式。
:::

1. 在[一对一通话示例代码](https://github.com/netease-im/Basic-Video-Call/tree/master/One-to-One-Video/NERtcSample-1to1-Windows_macOS-Qt)或[多人通话示例代码](https://github.com/netease-im/Basic-Video-Call/tree/master/Group-Video/NERtcSample-GroupVideoCall-Win-Mac-QT)下载页面下载需要的 Demo 源码工程。

2. 在源码工程所在文件夹下新建文件夹 `nertc_sdk_mac`。

3. 在 [SDK 下载中心](http://yunxin.163.com/g2-sdk-demo)获取最新版本的 NERTC SDK，将解压后的文件 `NEFundation_Mac.xcframework`、`nertc_sdk_Mac.xcframework` 拷贝到文件夹 `nertc_sdk_mac`下。

4. 开启一个终端，执行以下命令生成 Xcode 工程。

    ```
    qmake -spec macx-xcode NERtcSample-1to1-Windows_Mac.pro
    ```
4. 使用 Xcode 打开工程，在 **Build Phases** 选项中单击 **Copy Files Phase**。
5. 在 **Copy Files** 页面中将 **Destination** 设置为 **Frameworks**，并添加 `NEFundation_Mac.xcframework` 与 `nertc_sdk_Mac.xcframework`。

6. 在 `nrtc_engine.h`中配置 Appkey。

    ```c++
    #define APP_KEY ""              // put your app key here, testing
    ```
7. 使用 Xcode 打开上述建立好的工程，在 `Info.plist` 中添加访问摄像头及麦克风设备的权限，单机运行即可运行程序。