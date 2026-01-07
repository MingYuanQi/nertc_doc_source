<!--keywords: 音视频通话,RTC,跑通,示例项目,Sample Code,点对点音视频通话,多人音视频通话 -->
本文介绍如何运行网易云信音视频通话适配鸿蒙应用的 Sample Code，方便您体验音视频相关功能，包括音频通话、视频通话以及进阶功能。

## <span id="开发环境">开发环境</span>

在开始运行示例项目之前，请确保开发环境满足以下要求：

| 环境要求 | 说明 |
| ---- | ---- |
| IDE | DevEco Studio 5.0.3.900 及以上版本。 |
| Harmony API 版本 | Level 为 12 或以上版本。 |
| Harmony SDK 版本 | HarmonyOS 5.0.0 Release 或以上版本。 |
| Hvigor 及依赖库 | 示例项目源码中使用的 Hvigor 版本为 5.8.5。 |
| Harmony 设备 | Harmony 系统 5.0.0.102 及以上版本的真机。<note type="note">由于模拟器缺少摄像头及麦克风能力，因此工程需要在真机运行，请确保已正确连接 鸿蒙 设备。</note> |

## <span id="前提条件">准备工作</span>

请确认您已完成以下操作：

- <a href="https://doc.yunxin.163.com/console/guide/TIzMDE4NTA?platform=console">创建应用并获取 App Key</a>。
- <a href="https://doc.yunxin.163.com/console/concept/zc3NDYzNzc?platform=console">开通音视频通话 2.0 服务</a>。
- 下载 <a href="https://github.com/netease-im/rtc_harmony_demo">NERTC 示例项目源码</a> 仓库至您本地工程。

    ::: note note
    示例项目需要在 **RTC 调试模式** 下使用，此时无需传入 Token。修改鉴权方式的方法请参考 <a href="https://doc.yunxin.163.com/nertc/quick-start/jI4NjcyMjk?platform=harmony">Token 鉴权</a>。调试模式建议只在集成开发阶段使用，请在应用正式上线前改回安全模式。
    :::

## <span id="快速跑通 Sample Code">Demo 调试</span>

::: note note
本地工程目录请使用英文路径，不要包含中文字符。
:::

1. 使用 DevEco studio 打开示例项目源码仓库。

3. 打开 `entry/src/main/ets/common/Config.ets` 文件，修改文件中存储您的账号 App Key，替换为您在准备工作阶段获取的 App Key，如下所示：

    ```TypeScript
    export namespace Config{
      export const APP_KEY: string = "请填入业务方自己申请的 RTC APP_KEY"
      export const APP_KEY_DEBUG: string = "请填入业务方自己申请的 RTC APP_KEY"
    }
    export default Config
    ```

    <img alt="image.png" src="https://yx-web-nosdn.netease.im/common/afaf9c8571eccc4617a46dd8badee299/WX20241209-150908.png" style="width:80%;border: 1px solid #BFBFBF;">

    ::: note note
    AppKey 前后需要加英文双引号。
    :::

3. 打开 RTC DEMO 配置签名，路径为 **File > Project Structure**。进入 **Project Structure** 页面后，在 **Project > Signing Configs** 路径下，勾选 **Automatically generate signature**。

    <img alt="image.png" src="https://yx-web-nosdn.netease.im/common/cf5979ff87184642708bededdc14e75e/image.png" style="width:80%;border: 1px solid #BFBFBF;">

4. 运行 Demo。

    1. 开启设备的 **开发者模式** 和 **USB 调试** 功能。将 Harmony 设备连接到开发电脑，在弹出的授予调试权限对话框中，**授予调试权限**，具体步骤请参考 [在硬件设备上运行应用](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V2/ide_debug_device-0000001053822404-V2)。

        ::: note note
        请确保 Harmony 设备已开启开发者模式、USB 调试，并允许通过 USB 安装应用。
        :::

    2. 单击 Run 按钮，编译并运行示例源码。

        <img alt="image.png" src="https://yx-web-nosdn.netease.im/common/e4d2b4738996a2323db04023d83a5279/image.png" style="width:80%;border: 1px solid #BFBFBF;">

        ::: note note
        首次编译示例源码时，如果没有对应的依赖库或者构建工具，DevEco Studio 会自动下载示例源码，可能需要较长时间，请耐心等待。
        :::

5. 体验音视频通话。

    音视频通话需要获取麦克风等权限，请在 Harmony 设备上单击允许应用获取相应权限。

    跑通后的界面类似如下图所示，详细教程请参考 [Demo 使用教程](https://github.com/netease-im/rtc_harmony_demo/blob/main/document/DEMOUSEAGE.md)

    <img alt="cc" src="https://yx-web-nosdn.netease.im/common/c4f12dfc81ae715f599c672fc0a9a6b0/676550.jpeg" style="width:30%; border: solid grey, 1px;">

## 常见问题

::: details 1. 编译时提示 ENOENT: no such file or directory, open '/Users/minga/code/ohos_sample/dependencies/hvigor-4.0.2.tgz'。

<img alt="hvigor 没有找到.png" src="https://yx-web-nosdn.netease.im/common/78c6a3deb8dc7f8071b50305986a3c79/NDK版本不匹配.png" style="width:100%">

**可能原因**

hvigor 文件不匹配。

**问题解决**

下载对应版本的 hvigor 货件，例如 hvigor-4.0.2.tgz hvigor-ohos-plugin-4.0.2.tgz rollup.tgz。

1. 在 DevEco Studio 的菜单栏 中增加 dependencies 将上述文件拷贝到 dependencies 文件夹下。
2. 在 hvigor-config.json5 中增加 hvigorVersion 和 dependencies 的描述，如下图。

    <img alt="安装 hvigor.png" src="https://yx-web-nosdn.netease.im/common/db98346be326323c346d7f506f9dcac7/安装NDK.png" style="width:100%">
:::

::: details 2. 手机连接电脑后，DevEco Studio 中没有出现对应的手机。
如果 harmony 设备连接电脑后，Deveco Studio 的 **Running Devices** 中没有出现对应的手机，可能原因如下：
- 您的数据线不支持连接存储。
- Harmony 设备没有开启 **开发者模式** 和 **USB 调试**，或者连接手机时，在弹出的授予调试权限对话框中，没有授予权限。
:::