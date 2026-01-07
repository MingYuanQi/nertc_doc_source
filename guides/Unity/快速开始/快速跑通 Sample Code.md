<!--keywords: 音视频通话,RTC,跑通,示例项目,Sample Code -->
您可以通过跑通 Sample Code，体验网易云信音视频通话功能，包括一对一音视频通话和多人音视频通话。

## <span id="前提条件">前提条件</span>
请确认您已完成以下操作：

- <a href="https://doc.yunxin.163.com/console/guide/TIzMDE4NTA?platform=console" target="_blank">创建应用并获取 App Key</a>。
- <a href="https://doc.yunxin.163.com/nertc/quick-start/TA0ODQ2NjI?platformId=120271" target="_blank">开通音视频通话 2.0 服务</a>。
- <a href="https://doc.yunxin.163.com/nertc/quick-start/TcwNTgzMjg" target="_blank">集成 SDK（音视频）</a>，其中需要**添加必要的设备权限**和**导出工程的权限**。

## <span id="示例项目结构">示例项目结构</span>

您可以前往 <a href="https://github.com/netease-im/Unity-RTC-Quickstart" target="_blank">Github</a> 查看需要下载的 Demo 源码工程。
```
Unity-RTC-Quickstart
|
|-- API-Examples                         //RTC API Examples，包括基本的登录、消息收发、聊天室等                 
|   |
|   |-- Examples                         //所有的示例        
|       |
|       |   |-- Basic                    //演示基本功能的示例代码
|               |
|               |   |-- JoinChannel      //演示 RTC 加入音视频房间的示例代码
|               |   |-- JoinMultiChannel //演示 RTC 加入多个音视频房间的的示例代码
|       |
|       |   |-- Advanced                 //演示高级功能的示例代码
|               |
|               |   |-- 3DAudio          //演示空间音效的示例代码
|               |   |-- MultiVideoChat   //演示多人音视频聊天的示例代码
|    |
|    |-- Utils                           //工具类
|    |-- Editor                          //编辑器设置目录
|        |
|        |   |-- Builder.cs              //演示各平台所需要的的编译设置
|    |
|    |-- Plugins
|        |
|        |   |-- Android                 //Android平台
|        |   |-- AndroidManifest         //AndroidManifest.xml
```

## <span id="快速跑通 Sample Code">快速跑通 Sample Code</span>

::: note note
在运行示例项目之前，请在云信控制台中为指定应用<a href="https://doc.yunxin.163.com/nertc/quick-start/TUxMjYzODc?platformId=50136#修改鉴权方式" target="_blank">开通调试模式</a>。调试模式建议只在集成开发阶段使用，请在应用正式上线前改回安全模式。
:::

1. 下载最新版本的 <a href="https://doc.yunxin.163.com/all/sdk-download?platform=unity" target="_blank">NERTC Unity SDK</a>，并解压到本地。
2. 把下载到的 SDK 文件 `com.netease.game.rtc-*.*.*.tgz` 放到 `Packages` 目录。
3. 打开 `Unity Editor` 的 `Package Manager`，单击左上角 `+` 图标，然后单击 `"Add Package from tarball..."`，再选中 `Packages` 目录下的 `com.netease.game.rtc-*.*.0.tgz` 文件，即可完成导入。
4. 导入 SDK 后，进入 **Unity-RTC-Quickstart** > **API-Examples** > **Assest** > **Examples** 路径，选择想要运行的场景的 `MainScene` 文件，单击 **Canvas**，给场景绑定的脚本组件填入 APP_KEY、TOKEN、CHANNEL_NAME、UID 以及其他必要的信息。
5. 编译并运行示例项目。

