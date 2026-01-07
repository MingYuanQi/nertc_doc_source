
<!--音视频通话, SEI -->

通过 NERTC SDK，您可以将包括音量信息在内的自定义信息作为 SEI 的一部分，封装在视频码流中，并将其发送至远端用户解码查看，可用于音画同步等场景。

## <span id="功能描述">功能描述</span>

在音视频流媒体应用中，用户的消息分发通道和音视频或直播通道是分开的，通常情况下难以保证消息与视频数据的同步性。NERTC 支持将时间戳等自定义数据作为流媒体补充增强信息 （SEI Supplemental Enhancement Information）的一部分，通过流媒体通道将其与视频内容打包在一起，发送给远端用户，以此实现文本数据与音视频内容的精准同步的目的。

SEI 一般用于以下场景：
- 在线教育场景，知识竞答等教育教学工具中需要老师的声音和需要作答的题目进行同步。
- 电商购物场景，需要将商品信息和主播的声音及画面进行同步。
- 泛娱乐场景的在线 KTV 玩法中，需要在合唱场景中歌词和声音及画面进行同步。

## <span id="注意事项">注意事项</span>

纯音频场景的 SEI 帧通过 Fake Video 形式发送，不涉及视频流数据计费。

## <span id="实现方法">实现方法</span>

### <span id="音视频直播场景">**音视频直播场景**</span>

**本端：**

1. 本端加入房间后，调用 <a href="https://doc.yunxin.163.com/nertc/api-refer/unity/doxygen/Latest/zh/html/classnertc_1_1_i_rtc_engine.html#ad6d3e215a57f74960760a15aca5b73e5" target="_blank">`EnableLocalVideo`</a> 方法开启视频流。
2. 视频流成功开启后，调用 <a href="https://doc.yunxin.163.com/nertc/api-refer/unity/doxygen/Latest/zh/html/namespacenertc.html#a7a12fc98f3512f91d5589629d857a818" target="_blank">`SendSEIMsg`</a> 接口发送 SEI 信息。

**远端：**

远端接收到本端发送的 SEI 信息后，触发 <a href="https://doc.yunxin.163.com/nertc/api-refer/unity/doxygen/Latest/zh/html/namespacenertc.html#a7a12fc98f3512f91d5589629d857a818" target="_blank">`OnRecvSEIMessage`</a> 回调。

### <span id="纯音频通话场景">**纯音频通话场景**</span>

纯音频通话场景下，如果需要发送 SEI 信息到远端，SDK 会自动采取 Fake Video 方案。实现过程如下：

1. 本端调用 <a href="https://doc.yunxin.163.com/nertc/api-refer/unity/doxygen/Latest/zh/html/namespacenertc.html#a7a12fc98f3512f91d5589629d857a818" target="_blank">`SendSEIMsg`</a> 方法发送 SEI 信息。

    此时 SDK 内部会自动生成一个 Fake Video 的视频流，并携带 SEI 信息发送至远端。Fake Video 的分辨率为 16×16，画面为纯黑色。

2. 远端接收到纯音频流下发送的 Fake Video 的信令通知，通过 <a href="https://doc.yunxin.163.com/nertc/api-refer/unity/doxygen/Latest/zh/html/namespacenertc.html#af8428b05da263292a7388aa4d5e4be74a16f68abd40dcf2f4a8380704147db452" target="_blank">`kNERtcVideoProfileFake`</a> 字段判断该视频流为 Fake Video。

3. 远端接收到本端发送的 SEI 信息后，触发 <a href="https://doc.yunxin.163.com/nertc/api-refer/unity/doxygen/Latest/zh/html/namespacenertc.html#a7a12fc98f3512f91d5589629d857a818" target="_blank">`OnRecvSEIMessage`</a> 回调。

::: note note
在纯音频通话场景下，通过发送 Fake Video 的方式实现 SEI 帧发送后，若需要开启视频流发送正常的视频数据，可以先调用 `EnableLocalVideo` 方法开启视频流，并调用 `SendSEIMsg` 方法继续发送 SEI 信息。此时 SDK 会自动判断之前是否开启了 Fake Video，若已开启则会自动关闭 Fake Video，然后再开启视频。
:::

## <span id="示例代码">示例代码</span>


```C#
//发送SEI数据
private void sendSEIMessage()
{
    string content = "Hello,World!";
    byte[] data = System.Text.Encoding.UTF8.GetBytes(content);
    int result = rtcEngine.SendSEIMsg(data, RtcVideoStreamType.kNERTCVideoStreamMain);
    if (result != (int)RtcErrorCode.kNERtcNoError)
    {
        //失败
    }
}
```