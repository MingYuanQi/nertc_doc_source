<!--keywords: 音视频通话,RTC,跑通,示例项目,Sample Code -->
您可以通过跑通 Sample Code，体验网易云信音视频通话功能，包括一对一音视频通话和多人音视频通话。

## <span id="前提条件">前提条件</span>
请确认您已完成以下操作：

- <a href="https://doc.yunxin.163.com/console/guide/TIzMDE4NTA?platform=console" target="_blank">创建应用并获取 App Key</a>。
- <a href="https://doc.yunxin.163.com/nertc/quick-start/jE2NDQ0MDA" target="_blank">开通音视频通话 2.0 服务</a>。
- <a href="https://doc.yunxin.163.com/nertc/quick-start/jYwNjM5ODA" target="_blank">集成 SDK（Windows）</a>。

## 开发环境要求
请确保开发环境满足以下要求：

- 开发环境：Microsoft Visual Studio 2017（推荐）及以上版本
- 操作系统：Microsoft Windows 7 及以上版本
- 编译器：Microsoft Visual C++ 2017 及以上版本

## <span id="快速跑通 Sample Code">快速跑通 Sample Code</span>

::: note note
在运行示例项目之前，请在云信控制台中为指定应用<a href="https://doc.yunxin.163.com/nertc/quick-start/TUxMjYzODc?platformId=50136#修改鉴权方式" target="_blank">开通调试模式</a>。调试模式建议只在集成开发阶段使用，请在应用正式上线前改回安全模式。
:::

1. 在<a href="https://github.com/netease-im/Basic-Video-Call/tree/master/One-to-One-Video/NERtcSample-1to1-Windows-Qt" target="_blank">一对一通话示例代码</a>或<a href="https://github.com/netease-im/Basic-Video-Call/tree/master/Group-Video/NERtcSample-GroupVideoCall-Win-Mac-QT" target="_blank">多人通话示例代码</a>下载页面下载需要的 Demo 源码工程。
2. 在源码工程所在文件夹下新建文件夹 `nertc_sdk_win`。
3. 在 <a href="https://yunxin.163.com/im-sdk-demo?category=nrtc" target="_blank">SDK 下载中心</a>获取最新版本的 NERTC SDK，将解压后的文件 `api/`、`dll/` 、`lib/` 拷贝到文件夹 `nertc_sdk_win`下。
4. 在 `nrtc_engine.h` 文件中配置 App Key。

    ```cpp
    #define APP_KEY ""
    ```

5. 编译并运行示例项目。
    ::: note note
    若您需要运行的是多人通话 Demo，需要在编译前切换文件编码方式为 GB2312。
    :::

