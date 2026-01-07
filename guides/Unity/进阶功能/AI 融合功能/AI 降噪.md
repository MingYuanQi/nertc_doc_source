<!--和互动直播对应文档完全相同，可直接替换-->

在音视频通话或互动直播场景中，如果环境中有持续的背景噪声，在一定程度上会影响通话体验，例如户外环境的车流噪声、室内环境的背景人声等等。NERTC SDK 为您提供网易云信自研 AI 算法降噪功能，可智能分析环境音成分，自动甄别并过滤环境噪声。开启 AI 降噪之后，在嘈杂的环境中可以针对背景人声、键盘声等非稳态噪声进行定向降噪，同时也会提升对于环境稳态噪声的抑制，保留更纯粹的人声。

## <span id="实现方法">实现方法</span>

调用 <a href="https://doc.yunxin.163.com/nertc/api-refer/unity/doxygen/Latest/zh/html/classnertc_1_1_i_rtc_engine.html#a2da3418c60937de3a2d4d8205787d405" target="_blank">`SetParameters`</a> 方法，将 `kNERtcKeyAudioProcessingAINSEnable` 参数设置为 `true`，表示开启 AI 降噪功能。

::: note note
加入房间前和通话过程中均可以启用降噪功能。
:::

## <span id="示例代码">示例代码</span>

```C#
//开启AI音频降噪
private void setParameters()
{
    var dict = new Dictionary<string, object>();
    dict[RtcConstants.kNERtcKeyAudioProcessingAINSEnable] = true;//打开AI降噪开关

    string parameters = Newtonsoft.Json.JsonConvert.SerializeObject(dict);
    int result = rtcEngine.SetParameters(parameters);
    if (result != (int)RtcErrorCode.kNERtcNoError)
    {
        //失败
    }
}
```