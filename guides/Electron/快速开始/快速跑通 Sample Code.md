您可以通过跑通 Sample Code，体验网易云信音视频通话功能，包括一对一音视频通话和多人音视频通话。

## <span id="前提条件">前提条件</span>

请确认您已完成以下操作：

- [创建应用并获取App Key](https://doc.yunxin.163.com/console/guide/TIzMDE4NTA?platform=console)。
- [开通音视频通话 2.0 服务](https://doc.yunxin.163.com/nertc/quick-start/TI2NzgyNTg?platform=electron)。
- [集成 SDK（Electron）](https://doc.yunxin.163.com/nertc/quick-start/TQ0NjY1MDM)。

## <span id="快速跑通 Sample Code">快速跑通 Sample Code</span>

::: note note
在运行示例项目之前，请在云信控制台中为指定应用[开通调试模式](https://doc.yunxin.163.com/nertc/quick-start/TQ0MTI2ODQ?platform=android#修改鉴权方式)。调试模式建议只在集成开发阶段使用，请在应用正式上线前改回安全模式。
:::

1. [联系商务经理](https://yunxin.163.com/bizQQWPA.html)获取示例项目或 Demo 源码工程。

2. 打开 VSCode 或其他 IDE，执行以下命令准备和编译工程。

  ```bash
  npm i
  ```

3. 编译成功后，执行以下命令运行 Demo。

  ```bash
  npm run dev
  ```

4. 在 Demo 页面上输入 Appkey、LogDirPath、Token、ChannelName 和 UID。

  测试调试时，建议使用调试模式，此时无需使用 Token。如有需要，请在云信控制台中为指定应用[开通调试模式](https://doc.yunxin.163.com/nertc/quick-start/TQ0MTI2ODQ?platform=android#修改鉴权方式)。

5. 单击 initialize 初始化 SDK，成功后单击 joinChannel 即可加入房间。