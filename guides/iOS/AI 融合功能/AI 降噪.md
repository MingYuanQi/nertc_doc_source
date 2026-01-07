<!--和互动直播对应文档完全相同，可直接替换-->

在音视频通话或互动直播场景中，如果环境中有持续的背景噪声，在一定程度上会影响通话体验，例如户外环境的车流噪声、室内环境的背景人声等等。NERTC SDK 为您提供网易云信自研 AI 算法降噪功能，可智能分析环境音成分，自动甄别并过滤环境噪声。开启 AI 降噪之后，在嘈杂的环境中可以针对背景人声、键盘声等非稳态噪声进行定向降噪，同时也会提升对于环境稳态噪声的抑制，保留更纯粹的人声。

::: note note
**自 V4.6.40 起**，AI 降噪功能支持**插件化**集成，若您需要使用该功能，请在集成时引入 AI 降噪库 `NERtcAiDenoise.xcframework`，具体集成方式请参考<a href="/docs/jcyOTA0ODM/TM5NzI5MjI" target="_blank">集成 SDK</a>。
:::

## <span id="实现方式">实现方式</span>

将 `setParameters` 接口的 `KNERtcKeyAudioAINSEnable` 参数设置为 `YES`，表示开启 AI 降噪功能。

## <span id="示例代码">示例代码</span>

```
NSMutableDictionary *paramDic = [NSMutableDictionary dictionaryWithObjectsAndKeys:@(YES), KNERtcKeyAudioAINSEnable, nil];
//其他参数配置需要根据实际需要添加到上面的paramDic中
[[NERtcEngine sharedEngine] setParameters:paramDic];
```