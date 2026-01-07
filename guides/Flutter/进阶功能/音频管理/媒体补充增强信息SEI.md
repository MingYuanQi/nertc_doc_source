<!--和互动直播对应文档不同，不能直接替换-->

通过 NERTC SDK，您可以将包括音量信息在内的自定义信息作为 SEI 的一部分，封装在视频码流中，并将其发送至远端用户解码查看，可用于音画同步等场景。

## <span id="功能描述">功能描述</span>

在音视频流媒体应用中，用户的消息分发通道和音视频或直播通道是分开的，通常情况下难以保证消息与视频数据的同步性。NERTC 支持将时间戳等自定义数据作为流媒体补充增强信息 （SEI Supplemental Enhancement Information）的一部分，通过流媒体通道将其与视频内容打包在一起，发送给远端用户，以此实现文本数据与音视频内容的精准同步的目的。

<!--直播场景中，互动直播 2.0 会自动将音视频房间中参与互动直播成员的 SEI 信息进行重新封装，通过网易云信自定义的 SEI 类型打包，推流至播放端，您可以通过网易云信播放器 SDK 自动解析视频流 SEI 中封装的自定义数据。<!--【only in 互动直播2.0文档】-->

SEI 一般用于以下场景：

- 在线教育场景，知识竞答等教育教学工具中需要老师的声音和需要作答的题目进行同步。
- 电商购物场景，需要将商品信息和主播的声音及画面进行同步。
- 泛娱乐场景的在线 KTV 玩法中，需要在合唱场景中歌词和声音及画面进行同步。

## <span id="注意事项">注意事项</span>

- 默认通过主流发送 SEI 信息。您也可以在调用 sendSEIMsg 时指定通过辅流发送 SEI，发送前需确保该通道为开启状态。
- 纯音频场景的 SEI 帧通过 Fake Video 形式发送，不涉及视频流数据计费。

## <span id="实现方法">实现方法</span>

### <span id="音视频直播场景">音视频直播场景</span>

本端：

1. 本端加入房间后，通过 [`enableLocalVideo`](https://doc.yunxin.163.com/nertc/references/flutter/dartdoc/Latest/zh/nertc/NERtcEngine/enableLocalVideo.html) 开启视频流。
2. 视频流成功开启后，调用 [`sendSEIMsg`](https://doc.yunxin.163.com/nertc/references/flutter/dartdoc/Latest/zh/nertc/NERtcEngine/sendSEIMsg.html) 接口发送 SEI 信息。

远端：

远端接收到本端发送的 SEI 信息后，触发 `onRecvSEIMsg` 回调。

### <span id="纯音频通话场景">纯音频通话场景</span>

纯音频通话场景下，如果需要发送 SEI 信息到远端，SDK 会自动采取 Fake Video 方案。实现过程如下：

1. 本端调用  [`sendSEIMsg`](https://doc.yunxin.163.com/nertc/references/flutter/dartdoc/Latest/zh/nertc/NERtcEngine/sendSEIMsg.html) 发送SEI信息。

    此时 SDK 内部会自动生成一个 Fake Video 的视频流，并携带 SEI 信息发送至远端。Fake Video 的分辨率为 16×16，画面为纯黑色。

2. 远端接收到本端发送的SEI信息后，触发 `onRecvSEIMsg` 回调。
    ::: note note
    若您使用的是 v4.6.0 及之前版本的 SDK，此时需要订阅该视频流，但无需展示该视频流画面。
    :::

当您在纯音频通话场景下，通过发送 Fake Video 的方式实现 SEI 帧发送后，如果需要开启视频流正常发送视频数据，可以通过 enableLocalVideo 开启视频流，并调用 sendSEIMsg 继续发送 SEI 信息。此时SDK会自动判断之前是否开启了 Fake Video，如果开启了就会关闭 Fake Video，然后再开启视频。



## <span id="示例代码">示例代码</span>


```
// 发送SEI , 默认通过主流来发送
//int ret = await _engine.sendSEIMsg(seiMsg);
//或者可以指定是通过主流还是辅流来发送SEI
int ret = await _engine.sendSEIMsg(seiMsg,streamType: NERtcVideoStreamType.sub);
if (ret !=  0) {
    showToast("SEI 发送失败 , ret : " + ret);
}

//接收SEI， [NERtcChannelEventCallback.onRecvSEIMsg(long userID, String seiMsg)]

@override
void onRecvSEIMsg(int userID, String seiMsg) {
    //to do
}

```
