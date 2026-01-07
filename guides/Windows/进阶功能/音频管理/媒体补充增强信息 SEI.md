
<!--和互动直播对应文档不同，不能直接替换 -->

通过 NERTC SDK，您可以将包括音量信息在内的自定义信息作为 SEI 的一部分，封装在视频码流中，并将其发送至远端用户解码查看，可用于音画同步等场景。

## <span id="功能描述">功能描述</span>

在音视频流媒体应用中，用户的消息分发通道和音视频或直播通道是分开的，通常情况下难以保证消息与视频数据的同步性。NERTC 支持将时间戳等自定义数据作为流媒体补充增强信息 （SEI Supplemental Enhancement Information）的一部分，通过流媒体通道将其与视频内容打包在一起，发送给远端用户，以此实现文本数据与音视频内容的精准同步的目的。

<!--直播场景中，互动直播 2.0 会自动将音视频房间中参与互动直播成员的 SEI 信息进行重新封装，通过网易云信自定义的 SEI 类型打包，推流至播放端，您可以通过网易云信播放器 SDK 自动解析视频流 SEI 中封装的自定义数据。<!--【only in 互动直播2.0文档】-->

SEI 一般用于以下场景：
- 在线教育场景，知识竞答等教育教学工具中需要老师的声音和需要作答的题目进行同步。
- 电商购物场景，需要将商品信息和主播的声音及画面进行同步。
- 泛娱乐场景的在线 KTV 玩法中，需要在合唱场景中歌词和声音及画面进行同步。

## <span id="注意事项">注意事项</span>

- [sendSEIMsg](https://doc.yunxin.163.com/nertc/api-refer/windows/doxygen/Latest/zh/html/classnertc_1_1_i_rtc_engine_ex.html#aea98675fa90088569a24e57a4edf145a) 方法默认通过主流发送 SEI 信息。您也可以在调用 [sendSEIMsg](https://doc.yunxin.163.com/nertc/api-refer/windows/doxygen/Latest/zh/html/classnertc_1_1_i_rtc_engine_ex.html#ac8e8af17dcb2d80d2e1e00a9d0bd4856) 时指定通过辅流发送 SEI，发送前需确保该通道为开启状态。
- 纯音频场景的 SEI 帧通过 Fake Video 形式发送，不涉及视频流数据计费。

## <span id="实现方法">实现方法</span>

### <span id="音视频直播场景">音视频直播场景</span>

本端：

1. 本端加入房间后，通过 [enableLocalVideo](https://doc.yunxin.163.com/nertc/api-refer/windows/doxygen/Latest/zh/html/classnertc_1_1_i_rtc_engine.html#a57782589b573cf02bbfa836dace811c1) 开启视频流。
2. 视频流成功开启后，调用 [sendSEIMsg](https://doc.yunxin.163.com/nertc/api-refer/windows/doxygen/Latest/zh/html/classnertc_1_1_i_rtc_engine_ex.html#aea98675fa90088569a24e57a4edf145a) 接口发送 SEI 信息。

远端：

远端接收到本端发送的 SEI 信息后，触发 [onRecvSEIMsg](https://doc.yunxin.163.com/nertc/api-refer/windows/doxygen/Latest/zh/html/classnertc_1_1_i_rtc_engine_event_handler_ex.html#a7ae085dd56871d5f27abc6498c02d6ac) 回调。

### <span id="纯音频通话场景">纯音频通话场景</span>

纯音频通话场景下，如果需要发送 SEI 信息到远端，SDK 会自动采取 Fake Video 方案。实现过程如下：

1. 本端调用 [sendSEIMsg](https://doc.yunxin.163.com/nertc/api-refer/windows/doxygen/Latest/zh/html/classnertc_1_1_i_rtc_engine_ex.html#aea98675fa90088569a24e57a4edf145a) 发送SEI信息。

    此时 SDK 内部会自动生成一个 Fake Video 的视频流，并携带 SEI 信息发送至远端。Fake Video 的分辨率为 16×16，画面为纯黑色。

2. 远端接收到纯音频流下发送的 Fake Video 的信令通知，通过 [kNERtcVideoProfileFake](https://doc.yunxin.163.com/nertc/api-refer/windows/doxygen/Latest/zh/html/namespacenertc.html#a80b32c4c5cf6f4a36dbe6650ae859058) 字段判断该视频流为 Fake Video。

    ::: note note
    - 若您使用的是 v4.6.0 及之前版本的 SDK，此时需要订阅该视频流，但无需展示该视频流画面。
    - 若您使用的是 v4.6.1 及之后版本的 SDK，不再需要关注该信令通知。
    :::

3. 远端接收到本端发送的SEI信息后，触发 [onRecvSEIMsg](https://doc.yunxin.163.com/nertc/api-refer/windows/doxygen/Latest/zh/html/classnertc_1_1_i_rtc_engine_event_handler_ex.html#a7ae085dd56871d5f27abc6498c02d6ac) 回调。

当您在纯音频通话场景下，通过发送 Fake Video 的方式实现 SEI 帧发送后，如果需要开启视频流正常发送视频数据，可以通过 enableLocalVideo 开启视频流，并调用 sendSEIMsg 继续发送 SEI 信息。此时 SDK 会自动判断之前是否开启了 Fake Video，如果开启了就会关闭 Fake Video，然后再开启视频。


## <span id="示例代码">示例代码</span>


```
//string到UTF8的格式转换
std::string StringToUTF8(const std::string &str) { 
    int nwLen = ::MultiByteToWideChar(CP_ACP, 0, str.c_str(), -1, NULL, 0);
    wchar_t *pwBuf = new wchar_t[nwLen + 1]; 
    ZeroMemory(pwBuf, nwLen * 2 + 2); 
    ::MultiByteToWideChar(CP_ACP, 0, str.c_str(), str.length(), pwBuf, nwLen); 
    int nLen = ::WideCharToMultiByte(CP_UTF8, 0, pwBuf, -1, NULL, NULL, NULL, NULL); 
    char *pBuf = new char[nLen + 1]; 
    ZeroMemory(pBuf, nLen + 1); 
    ::WideCharToMultiByte(CP_UTF8, 0, pwBuf, nwLen, pBuf, nLen, NULL, NULL); 
 
    std::string strRet(pBuf); 
    delete[] pwBuf; 
    delete[] pBuf; 
    pwBuf = NULL; 
    pBuf = NULL; 
 
    return strRet; 
} 

// 首先获取到引擎对象 engine
nertc::IRtcEngineEx* nrtc_engine_ = ...

//subStream 为true表示辅流，false表示主流
NERtcStreamChannelType streamChannelType = subStream ? kNERtcStreamChannelTypeSubStream : kNERtcStreamChannelTypeMainStream; 

//SEI message，需要转换成UTF8格式
std::string strSEIInfo = StringToUTF8(strSEIMsg); 

//发送SEI消息
int res = nrtc_engine_->sendSEIMsg(reinterpret_cast<const char*>(strSEIInfo.c_str()), strSEIInfo.size()+1, streamChannelType); 

//消息打印
ShowLogInfo("ChannelType:%s, Send SEI msg:%s\n", subStream ? "kNERtcStreamChannelTypeSubStream" : "kNERtcStreamChannelTypeMainStream", strSEIInfo.c_str());
```

<!--## <span id="互动直播视频流的 SEI 格式">互动直播视频流的 SEI 格式</span>

在互动直播场景中，互动直播 2.0 服务端转码推流时，在转码后的 H264/H265 的 SEI 中增加当前视频的编码信息，其中包含客户端 SDK 上传的自定义 SEI 信息，封装类型为网易云信自定义的 SEI 类型。您可以通过网易云信播放器 SDK 自动解析视频流 SEI 中封装的自定义数据。

### <span id="SEI 格式">SEI 格式</span>

SEI 的格式为 JSON 格式的字符串，例如：

```
{
    "canvas":{
        "w":640,
        "h":360,
        "bgnd":"#000000"
    },
    "regions":[
        {
            "uid":1,
            "alpha":1,
            "zorder":1,
            "volume":50,
            "x":0,
            "y":0,
            "w":320,
            "h":360
        },
        {
            "uid":2,
            "alpha":1,
            "zOrder":1,
            "volume":89,
            "x":320,
            "y":0,
            "w":320,
            "h":360
        }
    ],
    "rtc_sei":[
        {
            "uid":1,
            "mainSei":"xxxxx",
            "subSei":"xxxxx"
        },
        {
            "uid":2,
            "mainSei":"xxxxx",
            "subSei":"xxxxx"
        }
    ],
    "ver":"20190611",
    "ts":1535385600000,
    "app_data":"",
    "extraInfo":"abc"
}
```
### <span id="字段说明">字段说明</span>

<table> 
<tr> 
<th width=20%><b>参数</b></th> 
<th width=20%><b> </b></th> 
<th width=50%><b>概述</b></th> 
</tr> 
<tr> 
<td rowspan=3>canvas</td>
<td>-</td> 
<td>画布信息。</td> 
</tr> 
<tr> 
<td>w</td> 
<td>画布的宽度，单位为像素。其值为推流端在 NERtcLiveStreamLayout 结构体中设置的 width 参数。</td> 
</tr> 
<tr> 
<td>h</td> 
<td>画布的高度，单位为像素。其值为推流端在 NERtcLiveStreamLayout 结构体中设置的 height 参数。</td> 
</tr> 
<tr> 
<td colspan=2>bgnd</td>
<td>bgnd：画布的背景颜色，格式为 RGB 定义下的十六进制整数。其值为推流端在 NERtcLiveStreamLayout 结构体中设置的 background_color 参数。</td> 
</tr> 
<tr> 
<td rowspan=9>regions</td> 
<td>-</td> 
<td>所有参与互动直播房间成员的信息（包含布局信息），为 region 的列表。
其值为推流端在 NERtcLiveStreamLayout 结构体中设置的 users 参数。</td> 
</tr> 
<tr> 
<td>uid</td> 
<td>该区域对应成员的 ID。其值为推流端在 NERtcLiveStreamUserTranscoding 结构体中设置的 uid 参数。</td> 
</tr> 
<tr> 
<td>alpha</td> 
<td>预留参数，暂未启用。</td> 
</tr> 
<tr> 
<td>zorder</td> 
<td>直播视频上用户视频帧的图层编号。</td> 
</tr> 
<tr> 
<td>volume</td> 
<td>该区域对成员的音量。</td> 
</tr> 
<tr> 
<td>x</td> 
<td>该区域在画布中对应的 x 坐标。其值为推流端在 NERtcLiveStreamUserTranscoding 结构体中设置的 x 参数。</td> 
</tr> 
<tr> 
<td>y</td> 
<td>该区域在画布中对应的 y 坐标。其值为推流端在 NERtcLiveStreamUserTranscoding 结构体中设置的 y 参数。</td> 
</tr> 
<tr> 
<td>w</td> 
<td>该区域的宽度，单位为像素。其值为推流端在 NERtcLiveStreamUserTranscoding 结构体中设置的 width 参数。</td> 
</tr> 
<tr> 
<td>h</td> 
<td>该区域的高度，单位为像素。其值为推流端在 NERtcLiveStreamUserTranscoding 结构体中设置的 height 参数。</td> 
</tr> <tr> 
<td rowspan=4>rtc_sei</td>
<td>-</td>
<td>参与互动直播的房间成员上传的 SEI。即其客户端 SDK 在 sendSEIMsg 中设置的 data 数据。</td> 
</tr> 
<tr> 
<td>uid</td>
<td>发送 SEI 的用户 ID。</td> 
</tr> 
<tr> 
<td>mainSei</td>
<td>客户端 SDK 在主流通道中封装的 SEI。</td> 
</tr> 
<tr> 
<td>subSei</td>
<td>客户端 SDK 在辅流通道中封装的 SEI。</td> 
</tr>
<tr> 
<td colspan=2>ver</td> 
<td>预留参数，暂未启用。</td> 
</tr> 
<tr> 
<td colspan=2>ts</td> 
<td>生成该信息时的 Unix 时间戳，单位为毫秒。</td> 
</tr> 
<tr> 
<td colspan=2>app_data</td>
<td>预留参数，暂未启用。</td> 
</tr> 
<tr> 
<td colspan=2>extraInfo</td>
<td>自定义的媒体补充增强信息。</td> 
</tr> 
</table>-->
