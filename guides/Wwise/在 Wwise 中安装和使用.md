本文介绍如何在 Audiokinetic Wwise 游戏应用设计工具中安装配置网易云信音视频通话 Wwise 插件（简称 NERTC Wwise 插件）。

## 插件版本

NERTC Wwise SDK 基于 **Wwise 2022.1.8.8316** 版本发布了对应的插件。

::: note notice
如果您有其他 Wwise 版本适配的需求，可以 [提交工单](https://app.yunxin.163.com/global/service/ticket/create) 联系网易云信技术支持工程师。
:::

## 前提条件

根据本文操作前，请确保您已经完成了以下步骤：

- 创建了 Wwise 设计工程。详细步骤请访问《Audiokinetic Wwise》[官方文档](https://www.audiokinetic.com/en/library/edge/?source=Help&id=setting_up_projects)。
- 创建了网易云信应用。详细步骤请参考 [创建应用并获取 AppKey](https://doc.yunxin.163.com/console/concept/TIzMDE4NTA?platform=console)。

## 第一步：安装插件

1. 前往下载 [NERTC Wwise SDK](https://doc.yunxin.163.com/nertc/resource?platform=wwise)。
2. 在本地安装 NERTC Wwise 插件。
3. 将以下插件添加到 Wwise 工程中：

    插件名称 | Wwise 插件分类 | 主要功能 | 配置方式说明 |
    ---- | ---- | ---- | ---- |
    Netease Audio Receive Source |  源插件 | 从网易云信 RTC 服务器接收同一房间的语音聊天。 |  [配置语音聊天以接收音频](#配置语音聊天以接收音频)
    Netease Audio Input Source | 源插件 | 录制玩家的语音，并对捕获到的信号进行预处理。 | -
    Netease Audio Session | 效果器插件 | 插入到 Audio Devices 层级下的设备对象上。主要功能是监控输出到 Audio Device 的音频，以便实现回声消除并设置服务器身份验证信息。 |  [配置 Netease Audio Session 插件](#NeteaseAudioSession)
    Netease Audio Send | 效果器插件 | 将本地录制的语音发送到 NERTC 服务器。 |  [配置语音聊天以发送音频](#配置语音聊天以发送音频)


<a id="NeteaseAudioSession"></a>

## 第二步：设置 Netease Audio Session 插件

为了确保正常运行，NERTC 必须监控最终信号来应用回声消除。为此，您需要在音频管线末端，配置一个效果器插件，即 Netease Audio Session 插件。此插件还可以为服务器设置身份验证信息。您需要将 Netease Audio Session 插件添加为 Audio Device 效果器。

### 1：添加为 Audio Devices 效果器

1. 在 **Audio** > **Audio Devices** 层级结构。
2. 在 System [System] 设备上，将 Netease Audio Session 插件添加 "Netease Audio Session" 效果器。

    这样可以确保用于回声消除模块的参考音频尽量接近麦克风采集到的当前系统音频输出信号。

    <img alt="image.png" src="https://yx-web-nosdn.netease.im/common/b0428b44d7c3402272b0bd1d680d1f70/image.png" style="width:80%;border: 1px solid #BFBFBF;">

### 2：配置 NERTC 身份验证信息和聊天室参数

在 Netease Audio Session 效果器插件的 Effects Settings 中设置 AppKey 和各项 User ID。双击 Netease Audio Session 效果器插件来修改设置。

- 每个工程都有一个唯一的 AppKey。由网易云信提供，用于启用插件。您需要在 [网易云信控制台](https://app.yunxin.163.com/global/home) 将此 AppKey 开启调试模式才能在 Wwise 工程中入会调试。
- ChannelName 用于在游戏中创建聊天室，以便所有参与者都可听到彼此说话。若要将玩家分到不同的团队，且团队之间不允许交谈，则可使用不同的 ChannelName。
- User ID 用户的身份标识，需要使用纯数字组合。每个用户需要一个唯一的 ID。

    <img alt="Netease Audio Send UI" src="https://yx-web-nosdn.netease.im/common/7dca93874125ad1ebce21fe895014ecb/2NERtcAudioSessionUI.png" style="width:80%;border: 1px solid #BFBFBF;">

### 3：配置语音聊天以发送音频

若要在 Wwise 中设置语音聊天，必须在工程中设置发送端和接收端。

您可以采用两种方式来将音频从某个玩家发送到其他玩家：

- 发送独立的音频，如玩家语音或特定音效。

    1. 在 Actor-Mixer Hierarchy 的 Default Work Unit 下，创建 Sound SFX 对象并将其命名为 NERtc Send。
    2. 选中 "NERtc Send" Sound SFX，在 Contents Editor 中单击 Add Source >>，然后选择 NERtc Audio Input Source 源插件

        <img alt="配置语音聊天以发送音频 Send" src="https://yx-web-nosdn.netease.im/common/f0ae5cabb9b8d34190dbaee8c5869106/2配置语音聊天以发送音频Send.png" style="width:80%;border: 1px solid #BFBFBF;">

- 发送多个声源的下混音频。

    有时，要将多个音频源混音到单个聊天信号中，然后再将其发送到聊天室中的其他玩家。例如，其中某个玩家可能会充当其他成员的 DJ。在这种情况下，音乐可能来自多个不同的声源。为此，最好将 Netease Audio Send 效果器插件设置到一个总线上而非单个音乐文件。

### 4：将混音后的语音作为单个语音音频流发送

1. 在 Master-Mixer Hierarchy 下，右键单击 Default Work Unit，然后依次单击 New Child > Bus。
2. 将新建总线的名称改为 NERtc Send Bus。

    <img alt="NERtc Send Bus" src="https://yx-web-nosdn.netease.im/common/b409446f234b22f1b675feb06a834baf/2NERtcSendBus.png" style="width:80%;border: 1px solid #BFBFBF;">

3. 在 Property Editor 中，选中 Effects 选项卡，然后在最后一个效果器插槽中插入 NERtc Send 效果器插件。

    <img alt="NERtc Send Audio" src="https://yx-web-nosdn.netease.im/common/935fc6ed7063561207eea6302ddb902a/2NERtcSendAudio.png" style="width:80%;border: 1px solid #BFBFBF;">

    所有路由到 NERtc Send Bus 或其子总线的音频对象都会与其他同时播放的声音一起混音。它们会被路由到同一总线，然后再发送到 NERTC 并被其他玩家接收。

4. 若要将音频发送到 NERtc Send Bus，请转到与声音或容器对应的 General Settings 选项卡，然后将 Output Bus 改为 NERtc Send Bus。

    <img alt="Set Audio Bus" src="https://yx-web-nosdn.netease.im/common/3d26dd373abb9fdf8a0f80e014c353bc/2SetAudioBus.png" style="width:80%;border: 1px solid #BFBFBF;">

### 5：配置语音聊天以接收音频

在配置工程以发送音频后，需要为音频添加接收端。若要为语音聊天创建接收端，必须创建新的使用 Netease Audio Receive Source 源插件的声音。

**配置音频接收端：**

1. 在 Actor-Mixer Hierarchy 的 Default Work Unit 下，创建新的 Sound SFX。例如，将名称设为 NERtc Receive。
2. 在 Contents Editor 中，单击 Add Source >> 并选择 Netease Audio Receive Source 源插件。

    <img alt="Audio Receive" src="https://yx-web-nosdn.netease.im/common/27e3dd5dd86ad91c4a02a5c025d44978/2AudioReceive.png" style="width:80%;border: 1px solid #BFBFBF;">

3. 选中 "NERtc Receive" Sound SFX 来将其加载到 Transport Control 中，然后单击 Play。若房间中有其他玩家，应可听到他们的声音。

## 第三步：准备工程

在创建用于发送和接收的声音并在 Wwise 工程中配置 Netease Audio Session 效果器及必要的 Appkey 和 userID 后，您需要创建若干 Event 并将其打包到一个 SoundBank 中，以便游戏应用使用相应服务。

### 1：创建 Event

对于每个 NERTC 相关声音，都要创建对应的 `Play` 和 `Stop` Event 来为游戏提供播和停的控制。另外，在游戏环境需要时，您还可创建 `Pause` 和 `Resume` Event。

- 为了能够接收语音，您可以创建以下四个 Event：

    "Receive" Event 名称 | 说明 |
    ---- | ---- |
    `Play_NERtc_Receive` | 开始接收语音 |
    `Stop_NERtc_Receive` | 停止接收语音 |
    `Pause_NERtc_Receive` | 暂停接收语音 |
    `Resume_NERtc_Receive` | 继续接收语音 |

- 为了能够发送语音，您可以创建以下四个 Event：

    "Send" Event 名称 | 说明 |
    ---- | ---- |
    `Play_NERtc_Send` | 开始发送语音 |
    `Stop_NERtc_Send` | 停止发送语音 |
    `Pause_NERtc_Send` | 暂停发送语音 |
    `Resume_NERtc_Send` | 继续发送语音 |

### 2：生成 SoundBank

将 NERTC 相关 Sound SFX 和 Event（如 `NERtc_Send` 和 `NERtc_Receive`）添加到 SoundBank 中，游戏才能予以加载和播放。