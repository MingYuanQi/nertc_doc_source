<!--keywords: 实时音视频,音视频通话,webrtc,RTC,集成 SDK -->
<!--和互动直播对应文档基本相同，替换产品名称后即可重用-->

本文介绍了在 Harmony 工程中集成网易云信 NERTC SDK 的集成步骤，属于实现实时音视频通话功能的准备工作。

## <span id="前提条件">开发环境</span>

在开始配置工程之前，请您准备以下开发环境：

- 华为 [DevEco Studio](https://developer.huawei.com/consumer/cn/deveco-studio/#download) 5.0.3.900 或以上版本。
- Harmony SDK API 等级 12 或以上。
- Harmony 系统 5.0.0.102 或以上版本的移动设备。
- Hvigor 5.8.5 以及以上版本。

<!-- 
## <span id="SDK 目录">SDK 目录</span>

SDK 的目录组成结构如下：

| 文件/文件夹名称 | 是否必选 | 说明 |
| --- | --- | ---- |
| nertc_sdk.har | 是 | 音视频库。 |
| libnertc_sdk.so | 是 | 音视频库。 |
| libgrowease.so | 是 | 智企库（自 V4.6.20 起提供）。 |
| libNERtcnn.so | 是（V5.3.1 及之后版本：否） | 神经网络库。 |
| libNERtcBeauty.so | 否 | 美颜库（自 V4.6.20 起提供，用于实现插件化）。 |
| libNERtcFaceDetect.so | 否 | 人脸检测库（自 V4.6.20 起提供，用于实现插件化）。 |
| libNERtcPersonSegment.so | 否 | 背景分割库（自 V4.6.20 起提供，用于实现插件化）。 |
| libNERtcAiDenoise.so | 否 | AI 降噪库（自 V4.6.40 起提供，用于实现插件化）。 |
| libNERtcAiHowling.so | 否 | AI 啸叫检测库（自 V4.6.40 起提供，用于实现插件化）。 |
| libNERtcFaceEnhance.so | 否 | 人脸增强库（自 V5.3.0 起提供，用于实现插件化）。 |
| libNERtcSuperResolution.so | 否 | 视频超分库（自 V5.3.0 起提供，用于实现插件化）。 |
| libNERtcVideoDenoise.so | 否 | 视频降噪库（自 V5.3.0 起提供，用于实现插件化）。 |
| libNERtcAudio3D.so | 否 | 空间音效库（自 V5.4.0 版本开始提供，用于实现插件化）。 | -->

## <span id="集成 SDK">集成 SDK</span>

### <span id="Ohpm 集成">Ohpm 集成（推荐）</span>

1. （可选）创建新项目。

    ::: details    您可以参考此步骤创建新项目，若是需要集成到已有的项目中，请忽略该步骤。
    1. 在 DevEco Studio 里，在顶部菜单依次选择 **File** > **New** > **Create Project** 新建工程，再依次选择 **Application** > **Empty Activity**，单击 **Next**。
    2. <a href="https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V2/create_new_project-0000001053342414-V2" target="_blank">创建 Harmony 项目</a> 成功后，DevEco Studio 会自动开始同步 hvigor, 您需要等同步成功后再进行下一步操作。
    :::

2. 在工程目录下的 entry 目录，执行 Ohpm 命令

    进入项目根目录，进入 entry 目录下，执行以下命令。

    ```
    ohpm install @nertc/nertc_sdk
    ```

3. 打开 DevEco studio。DevEco studio 会自动同步（sync）。

### <span id="手动集成">手动集成</span>

1. （可选）创建新项目。

    ::: details    您可以参考此步骤创建新项目，若是需要集成到已有的项目中，请忽略该步骤。
    1. 在 DevEco Studio 里，在顶部菜单依次选择 **File** > **New** > **Create Project** 新建工程，再依次选择 **Application** > **Empty Activity**，单击 **Next**。
    2. <a href="https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V2/create_new_project-0000001053342414-V2" target="_blank">创建 Harmony 项目</a> 成功后，DevEco Studio 会自动开始同步 hvigor, 您需要等同步成功后再进行下一步操作。
    :::

2. 前往 <a href="https://doc.yunxin.163.com/nertc/resource?platform=harmonyos" target="_blank">SDK 下载页面</a> 获取当前最新版本 SDK，或 [提交工单](https://app.yunxin.163.com/global/service/ticket/create) 联系网易云信技术支持工程师获取对应版本的 SDK。

3. 解压后将对应的文件拷贝至项目路径中。
    <table border="1">
    <tr>
      <th width="30%"><b>文件/文件夹</b></th>
      <th width="50%"><b>项目路径</b></th>
    </tr>
    <tr>
      <td>nertc_sdk.har</td>
      <td>/entry/src/main/libs</td>
    </tr>
    </table>

    ![image.png](https://yx-web-nosdn.netease.im/common/08a5f5833a9ec63c98724aff74c92968/image.png)

    ::: note note
    若无对应文件夹，您需要在对应路径下新建文件夹。
    :::

4. 打开 DevEco studio 在 **entry/oh-package.json5** 文件中设置 **dependencies** 路径。

     ```json
    "dependencies": {
        '@nertc/nertc_sdk': "file:./src/main/libs/nertc_sdk.har"
    }
    ```
   之后可以在工程文件中调用 SDK 中的相关函数。

## <span id="添加权限">添加权限</span>

打开 `entry/src/main/module.json5` 文件，添加必要的设备权限。

权限说明如下表所示：

| 必要性 | 获取的权限 | 使用目的 |
| --- | ---- | ---- |
| 必要权限 | 访问网络权限（INTERNET） | SDK 基本功能都需要在联网的情况下才可以使用 |
| ^^ | 网络连接状态（GET_NETWORK_INFO） | 获取网络的类型、拥有的能力等 |
| ^^ | 麦克风（MICROPHONE） | 音视频通话时，用于采集声音 |
| ^^ | 照相机（CAMERA） | 音视频通话时，用于采集视频 |

配置文件示例：

```json5
"requestPermissions": [
   {
      "name": "ohos.permission.CAMERA",
      "usedScene": {
      "abilities": [
        "FormAbility"
      ],
      "when":"always"
      }
   },
   {
      "name": "ohos.permission.MICROPHONE",
      "usedScene": {
      "abilities": [
        "FormAbility"
      ],
      "when":"always"
      }
   }
   {
      "name": "ohos.permission.GET_NETWORK_INFO",
      "usedScene": {
      "abilities": [
        "FormAbility"
      ],
      "when":"always"
      }
   },
   {
      "name": "ohos.permission.INTERNET",
      "usedScene": {
        "abilities": [
      "FormAbility"
      ],
      "when":"always"
      }
   }
]
 ......//App 需要的其他设备权限
```

## <span id="后续步骤">下一步</span>

参考教程 <a href="https://doc.yunxin.163.com/nertc/quick-start/DY0OTIwOTM" target="_blank">实现音视频通话</a>。