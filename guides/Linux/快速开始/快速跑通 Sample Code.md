您可以通过跑通 Sample Code，体验网易云信一对一音视频通话功能。

## <span id="前提条件">前提条件</span>

请确认您已完成以下操作：

- [创建应用并获取 App Key](https://doc.yunxin.163.com/console/guide/TIzMDE4NTA?platform=console)。
- [开通音视频通话 2.0 服务](https://doc.yunxin.163.com/nertc/quick-start/jM2MTQ0MjY)。
- [集成 SDK（Linux）](https://doc.yunxin.163.com/nertc/quick-start/zk2Njk3NTY)。

## <span id="操作步骤">操作步骤</span>

::: note notice
在运行示例项目之前，请在云信控制台中为指定应用[开通调试模式](https://doc.yunxin.163.com/nertc/quick-start/TQ0MTI2ODQ?platform=android#修改鉴权方式)。调试模式建议只在集成开发阶段使用，请在应用正式上线前改回安全模式。
:::

1. 在[一对一通话示例代码](https://github.com/netease-im/Basic-Video-Call/tree/master/One-to-One-Video/NERtcSample-1to1-Linux-Qt/NEVideoCall-Linux)下载页面下载需要体验的 Demo 源码工程。
2. 在 `rtcengine.h` 文件中配置 App Key。

    ```cpp
    #define APP_KEY ""
    ```

3. 通过 Qt creator 打开 `NEVideoCall-Linux.pro` 文件，重新构建后即可运行程序。

