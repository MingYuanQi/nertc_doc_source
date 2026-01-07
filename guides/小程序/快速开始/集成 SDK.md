<!--和互动直播对应文档基本相同，替换产品名称后即可重用-->

本文为您介绍了小程序端集成 SDK 的操作步骤，帮助您快速集成 SDK 并实现实时音视频通话的基本功能。

## 注意事项

微信官方在其开发者社区增加了《小程序用户隐私保护指引》等条款。在小程序正式版本发布之前，您需要加入该指引中设备的说明。如果没有进行配置，可能会导致正式版本的摄像头和麦克风无法开启。

详细说明和配置方法请参考 [配置小程序用户隐私保护指引](https://developers.weixin.qq.com/doc/oplatform/Third-party_Platforms/2.0/product/privacy_setting.html)。

:::note note
由于 NERTC 小程序 SDK 是基于腾讯微信和 QQ 平台搭建的，小程序的固有问题可能会影响您的使用体验。例如，在音视频通话场景中，如果将小程序组件的 `live-player` 组件中的 `mode` 字段设置为 RTC，在部分 iPhone 机型中可能会造成通话时音质差等声音问题。

随着小程序平台的升级迭代，该问题可能会逐步解决，但当下网易云信推荐您采用临时方案规避。即设置 `live` 直播模式、并将缓冲区设置为推荐范围。该方案会致使通话延时增大到 0.8～1s，但不影响正常使用。操作步骤：

1. 将 `live-player` 组件中的 `mode` 字段设置为默认值 `live`。
2. 将 `live-player` 组件中的 `min-cache` 设置为 0.2s，将 `max-cache` 设置为 0.8s。
:::

## <span id="环境要求">环境要求</span>

请确保您的开发环境符合以下环境要求：

- 微信小程序：
    - 微信 App iOS 最低版本要求：7.0.9
    - 微信 App Android 最低版本要求：7.0.8
    - 小程序基础库最低版本要求：2.10.0
    - 已安装最新版本的 [微信开发者工具](https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html)。
    - 由于微信开发者工具不支持原生组件（即 <[live-pusher](https://mp.weixin.qq.com/debug/wxadoc/dev/component/live-pusher.html)> 和 <[live-player](https://mp.weixin.qq.com/debug/wxadoc/dev/component/live-player.html)> 标签），需要在真机上进行运行体验。

- QQ 小程序：

    - QQ App 推荐版本：8.8.3 和 8.8.5
    - 小程序基础库最低版本要求：v1.28.0
    - 已安装最新版本的 [QQ 小程序开发者工具](https://q.qq.com/wiki/tools/devtool/)。
    - 已安装 QQ 的移动端设备以供调试和运行体验。

## 准备工作

根据本文操作前，请确保您已经完成了以下设置：

- 已参考 [入门操作流程](https://doc.yunxin.163.com/nertc/concept/DkyMDM2Mzk?platform=client)，在网易云信控制台中创建应用、获取 App Key，且已为该应用开通了音视频通话 2.0 产品。
- 微信小程序：
    - 已注册微信小程序账号，并通过企业认证，并在 [微信公众平台](https://mp.weixin.qq.com/) 的 **开发** > **接口设置** 中，开启 **实时播放音视频流** 和 **实时录制音视频流** 权限。仅 [指定类目](https://developers.weixin.qq.com/miniprogram/dev/component/live-pusher.html) 的应用可以开通小程序推拉流标签。

       <img alt="开通组件权限.jpg" src="https://yx-web-nosdn.netease.im/common/177707d81729a491d9d10cdf82c66aca/开通组件权限.jpg" style="width:80%;border: 1px solid #BFBFBF;">

    - 已在微信小程序中创建微信的 [live-pusher](https://mp.weixin.qq.com/debug/wxadoc/dev/component/live-pusher.html) 组件和 [live-player](https://mp.weixin.qq.com/debug/wxadoc/dev/component/live-player.html) 组件，分别实现音视频播放和音视频录制功能。
- QQ 小程序：
    - 注册 QQ 小程序开发者平台，并通过资质审核。详细说明请参考 [QQ 小程序官方文档](https://q.qq.com/wiki/)。
    - 已在 QQ 小程序中创建 [live-pusher](https://q.qq.com/wiki/develop/miniprogram/component/media/live-pusher.html) 组件和 [live-player](https://q.qq.com/wiki/develop/miniprogram/component/media/live-player.html) 组件，分别实现音视频播放和音视频录制功能。
    - 仅指定类目的应用可以使用 `live-pusher` 和 `live-player` 组件，请确认应用已通过小程序的类目审核。

## <span id="配置白名单并打开端口">配置白名单</span>

如果您的网络环境部署了防火墙，请在小程序管理后台，将以下域名及对应端口添加到域名白名单中。

域名：

```
# http 接口
https://nrtc.netease.im
https://webrtcgwcn.netease.im
https://webrtcgwhz.netease.im
https://statistic.live.126.net
# websocket 接口
wss://webrtcgwcn.netease.im
wss://webrtcgwhz.netease.im
```
端口：

<table border=1>
    <tr>
        <th width=20%><b>目标端口</b></th>
        <th width=10%><b>协议</b></th>
        <th width=10%><b>操作</b></th>
    </tr>
    <tr>
        <td>80、443
        </td>
        <td>TCP
        </td>
        <td>允许
        </td>
    </tr>
    <tr>
        <td>30000 ~ 40000
        </td>
        <td>UDP
        </td>
        <td>允许
        </td>
    </tr>
</table>

以下为微信小程序的配置步骤举例：

在 [微信公众平台](https://mp.weixin.qq.com/) 的 **开发** > **开发管理** > **开发设置** > **服务器域名** 中，设置 **request 合法域名** 和 **socket 合法域名**。

<img alt="配置白名单.png" src="https://yx-web-nosdn.netease.im/common/9a6216ce0df8622bc63d67c895930470/配置白名单.png" style="width:80%;border: 1px solid #BFBFBF;">

## <span id="集成 SDK">集成 SDK</span>

1. 请登录 [网易云信 SDK 下载中心](https://doc.yunxin.163.com/nertc/sdk-download) 下载 NERTC 微信小程序 SDK。
2. 引入 SDK。

    ```JavaScript
    import YunXinMiniappSDK from '../../sdk/NERTC_Miniapp_SDK_v4.6.11.js'
    ```

    ::: note note
    此处版本号仅为示例，推荐使用最新版本。
    :::

## <span id="后续步骤">下一步</span>

[实现音视频通话（小程序）](https://doc.yunxin.163.com/nertc/quick-start/jY0NzUxMDE)