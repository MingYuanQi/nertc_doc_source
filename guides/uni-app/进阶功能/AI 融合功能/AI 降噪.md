<!--和互动直播对应文档完全相同，可直接替换-->

在音视频通话或互动直播场景中，如果环境中有持续的背景噪声，在一定程度上会影响通话体验，例如户外环境的车流噪声、室内环境的背景人声等等。NERTC SDK 为您提供网易云信自研 AI 算法降噪功能，可智能分析环境音成分，自动甄别并过滤环境噪声。开启 AI 降噪之后，在嘈杂的环境中可以针对背景人声、键盘声等非稳态噪声进行定向降噪，同时也会提升对于环境稳态噪声的抑制，保留更纯粹的人声。



## <span id="实现方式">实现方式</span>

将 `setParameters` 接口的 `keyAudioAINSEnable` 参数设置为 `true`，表示开启 AI 降噪功能。

## <span id="示例代码">示例代码</span>

```
this.engine.setParameters({
    keyAudioAINSEnable: true
});
```

## API 参考
| **方法** | **功能描述**|
|:--|:--|
|<a href="https://doc.yunxin.163.com/nertc/api-refer/uniapp/typedoc/Latest/zh/html/modules/nertc.nertc-1.html#setparameters" target="_blank">`setParameters`</a>|此接口提供技术预览或特别定制功能(如AI降噪)。	|
