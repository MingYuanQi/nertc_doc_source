在社交娱乐行业的互动场景中，美声、变声、降噪功能起着重要作用。本文不仅介绍了如何使用网易云信音视频通话的 AI 音效插件，实现 AI 降噪和美声变声结合的功能，更提供多种典型业务场景的完整代码实现方案。

## 应用场景

网易云信音视频通话 SDK（简称 NERTC SDK）支持设置多种预设的美声与变声音效，您也可以通过设置本地语音音效均衡或混响来达到自定义的人声效果，增加场景气氛。在多人语聊或 K 歌房场景中，AI 音效能够对主播或参与者的声音进行美化，营造更加娱乐化的氛围，提高互动体验。

在音视频通话或互动直播场景中，背景噪声可能会干扰通话质量，例如户外的车流声或室内的背景人声。AI 音效插件使用网易云信自主研发的 AI 算法，具备降噪功能。它能智能识别环境中的各种声音，过滤掉环境噪声对通话造成的干扰。开启 AI 降噪功能后，可将背景中的人声、键盘声等不稳定噪音进行降噪处理，同时提高对稳定噪声的抑制，使人声更加清晰纯净。

## 变更记录

以下为 NERTC SDK AI 音效插件的变更记录，详细记录请参考 [更新日志](https://doc.yunxin.163.com/nertc/concept/zU5NzI3NTM?platform=client)，您可以前往 <a href="https://doc.yunxin.163.com/nertc/resource?platform=all" target="_blank">网易云信 SDK 下载中心</a> 获取最新版本 SDK。

- 从 5.8.25 版本起，新增 `localStream.on('howling-suppression-result')` 事件，用于接收 AI 降噪后的音频中仍残留的啸叫事件。
- 从 5.8.0 版本起，支持远端音频流混流后变声功能，优化音频后处理的美声和变声效果。使用方法请参考下文 [远端流混流后变声](#setAudioEffect)。
- 从 5.6.41 版本起，如果使用 `await rtc.localStream.registerPlugin()` 方式注册 AI 音效插件，您需要在调用 `registerPlugin()` 之前绑定插件事件。
- 从 5.6.10 版本起，NERTC SDK 支持远端流的美声变声能力。使用方法请参考下文 [远端流示例代码](#remoteStreamSample)。
- 从 5.5.11 版本起，NERTC SDK 支持美声变声混响。注册插件时 AI 降噪使用的 `key` 需要从 `AIDenoise` 替换为 `AIAudioEffects`，其他接口使用方式不变。
- 从 4.6.25 版本起，NERTC SDK 支持 AI 降噪。

## 浏览器版本

AI 音效功能需要的浏览器最低版本兼容情况如下：

- Google Chrome 浏览器 72 及以上版本
- Apple Safari 浏览器 15.4 及以上版本
- Mozilla Firefox 浏览器最新（120）版本
- Microsoft Edge 浏览器最新（119）版本

## 注意事项

- [`setAudioEffect`](https://doc.yunxin.163.com/nertc/references/web/typedoc/Latest/zh/html/interfaces/stream.stream-1.html#setaudioeffect) 方法不支持预先设置，必须在触发 `audio-effect-enabled` 事件回调之后调用才能生效。
- 如果调用了 [`disableAudioEffect`](https://doc.yunxin.163.com/nertc/references/web/typedoc/Latest/zh/html/interfaces/stream.stream-1.html#disableaudioeffect) 方法关闭音效，后续再次调用 [`enableAudioEffect`](https://doc.yunxin.163.com/nertc/references/web/typedoc/Latest/zh/html/interfaces/stream.stream-1.html#enableaudioeffect) 时，需要重新设置美声变声效果，之前的设置不会被保留。
- 在开启伴音 [startaudiomixing](https://doc.yunxin.163.com/nertc/references/web/typedoc/Latest/zh/html/interfaces/stream.stream-1.html#startaudiomixing) 替换麦克风采集的音频流后，不支持开启 AI 音效，两者功能互斥。

## 本端流 AI 音效

### AI 降噪功能

1. 调用 <a href="https://doc.yunxin.163.com/nertc/references/web/typedoc/Latest/zh/html/modules/nertc.nertc-1.html#createstream" target="_blank">`createStream`</a> 方法创建并返回一个本地音视频流对象。
    :::note notice
    - 调用 `createStream` 创建媒体流之前，需要通过 `getDevices` 方法获取麦克风和摄像头设备的 deviceId。详细说明请参考 <a href="https://doc.yunxin.163.com/nertc/guide/zk3NjE3NTA?platform=web" target="_blank">音视频设备检测</a>。
    - 必须先初始化并发布本地流，再注册插件。
        ```JavaScript
        await rtc.localStream.init()
        await rtc.localStream.publish()
        ```
    :::

2. 调用 <a href="https://doc.yunxin.163.com/docs/interface/NERTC_SDK/Latest/Web/api/interfaces/stream.stream-1.html#registerplugin" target="_blank">`registerPlugin`</a> 方法并配置 `pluginOptions` 参数注册 AI 音效插件，其中 `pluginOptions = {key: 'AIAudioEffects', pluginUrl?: string, pluginObj?: Object, wasmUrl: string}`，相关字段的具体说明如下：

    <div style="width:100px">参数</div> | 是否必选 | 描述
    ---- | ---- | ----
    `key` | 是 | `AIAudioEffects`，必填，表示 AI 音效插件。
    `pluginUrl` | 否 | 插件的 CDN 地址，与 `pluginObj` 参数互斥，两者必须选择其中一个。支持自定义 URL 地址。
    `pluginObj` | 否 | 插件对象，与 `pluginUrl` 参数互斥，两者必须选择其中一个。请通过 NPM 方式安装，具体安装方式请参考下文 [**说明**](#installPluginObj)。
    `wasmUrl` | 是 | 插件依赖的 wasm 文件地址。官网的 SDK 包以及 NPM 包均提供原文件，可以部署到您自己的本地服务器中。

    ::: note note
    - AI 音效插件提供 simd 版本和非 simd 版本，建议使用 simd 版本以获得更佳性能。如果您的浏览器不支持 simd 版本，请使用非 simd 版本的插件。要检测您的浏览器是否支持 simd，可以参考 <a href="https://github.com/GoogleChromeLabs/wasm-feature-detect" target="_blank">`wasm-feature-detect.git`</a> 工具。
    - 必须传入 `pluginUrl` 和 `pluginObj` 中的一项，两者不能同时使用，也不能同时为空。
    - <span id="installPluginObj">**安装 `pluginObj` 的代码如下**</span>：

        ```Bash
        //第一步，安装 nertc-web-sdk
        npm install nertc-web-sdk
        //第二步，导入 AI 音效插件
        import AIAudioEffects from 'nertc-web-sdk/NERTC_Web_SDK_AIAudioEffects'
        ```

    - 如果您选择通过 NPM 安装插件，安装完成后可在 `nertc-web-sdk/wasm` 路径下的文件夹中找到 `NERTC_Web_SDK_AIAudioEffects_simd.wasm` 和 `NERTC_Web_SDK_AIAudioEffects_nosimd.wasm` 文件，您需要将这些文件部署到您自己的服务器中，然后将部署后的文件地址通过 `wasmUrl` 参数传递给 SDK。
    :::

3. 在本端监听 `on('plugin-load')` 和 `on('plugin-load-error')` 事件，以判断 AI 音效插件是否加载成功。
   本端监听 `on('ai-denoise-enabled')` 事件，以判断 AI 降噪功能已经启动，在该事件中可以调用 <a href="https://doc.yunxin.163.com/nertc/references/web/typedoc/Latest/zh/html/interfaces/stream.stream-1.html#setvoicegate" target="_blank">`setVoiceGate`</a> 方法设置背景人声消除阈值

    ::: note note
    如果插件注册失败触发了 `on('plugin-load-error')` 回调，请关注返回的 `event` 结构中的 `msg` 字段，常见错误信息及解决方法：
    - **`Load {wasmUrl} error`**：wasm 文件加载失败，请检查 URL 地址是否正确，以及服务器是否配置了正确的 MIME 类型。
    - **`unsupport plugin {key}`**：不支持该插件，请检查 `key` 参数是否为 `'AIAudioEffects'` 且 `pluginUrl/pluginObj` 参数值是否正确。
    - **`Load plugin ${pluginUrl} error`**：插件 JS 文件加载失败，请检查 URL 地址是否正确。
    :::

4. 在接收到 `plugin-load` 回调事件后，调用 <a href="https://doc.yunxin.163.com/nertc/references/web/typedoc/Latest/zh/html/interfaces/stream.stream-1.html#enableaidenoise" target="_blank">`enableAIDenoise`</a> 方法（可以设置 `howlingSuppressionEnable` 参数开启啸叫检查）来启用 AI 降噪功能。


5. 如果需要取消 AI 降噪效果，请调用 <a href="https://doc.yunxin.163.com/nertc/references/web/typedoc/Latest/zh/html/interfaces/stream.stream-1.html#disableaidenoise" target="_blank">`disableAIDenoise`</a> 方法关闭 AI 降噪进程。

6. 如果需要完全销毁 AI 音效插件并释放资源，请调用 <a href="https://doc.yunxin.163.com/nertc/references/web/typedoc/Latest/zh/html/interfaces/stream.stream-1.html#unregisterplugin" target="_blank">`unregisterPlugin(pluginKey)`</a> 方法。

### 美声和变声功能

网易云信 NERTC SDK 为您提供多种类型的预设人声效果，以满足您在不同场景下的美声与变声需求。如果预设的美声与变声效果无法满足您的需求，也可以通过调整音调、音效均衡和混响参数，实现自定义的人声效果。

调用 [`setAudioEffect(type, value)`](https://doc.yunxin.163.com/nertc/references/web/typedoc/Latest/zh/html/interfaces/stream.stream-1.html#setaudioeffect) 方法可以设置各类音效，支持以下美声，变声和自定义音效：

| type | value | 说明 |
| ---- | ---- | ---- |
| 0（变声） | 0 | 关闭变声 |
| ^^ | 1 | 机器人 |
| ^^ | 2 | 巨人 |
| ^^ | 3 | 恐怖 |
| ^^ | 4 | 成熟 |
| ^^ | 5 | 男变女 |
| ^^ | 6 | 女变男 |
| ^^ | 7 | 男变萝莉 |
| ^^ | 8 | 女变萝莉 |
| 1（美声） | 0 | 关闭美声 |
| ^^ | 1 | 低沉 |
| ^^ | 2 | 圆润 |
| ^^ | 3 | 清澈 |
| ^^ | 4 | 磁性 |
| ^^ | 5 | 录音棚 |
| ^^ | 6 | 天籁 |
| ^^ | 7 | KTV |
| ^^ | 8 | 悠远 |
| ^^ | 9 | 教堂 |
| ^^ | 10 | 卧室 |
| ^^ | 11 | Live |
| Pitch(音调) | [0.5, 2]，步长 0.1 | 默认值 = 1（正常音调），值越小音调越低，值越大音调越高。 |
| EQ | Array(10)，[-15,15]，步长 1 | 默认值为全 0，分别代表 10 个频带的增益，对应的中心频率分别为 31、62、125、250、500、1k、2k、4k、8k 和 16k Hz。 |
| Reverb（混响） | Object | 以下每个参数的步长均为 0.1。 |
    wetGain | Number | 湿信号增益。该参数单位为分贝（dB），取值范围为 0 ~ 1，默认值为 0.0，值越大混响效果越明显。 |
    dryGain | Number | 干信号增益。该参数单位为分贝（dB），取值范围为 0 ~ 1，默认值为 1.0，值越小原始声音越弱。 |
    damping | Number | 混响阻尼，用于设置混响的衰减程度，阻尼越大表示衰减越快。取值范围为 0 ~ 1，默认值为 1.0。 |
    decayTime | Number | 持续强度，用于设置混响的拖尾长度。单位为秒，取值范围为 0.1 ~ 20，默认值为 0.1，值越大余音越长。 |
    roomSize | Number | 房间大小，用于设置模拟的 **房间** 大小，房间越大表示混响越强。取值范围为 0.1 ~ 2，默认值为 0.1。 |
    preDelay | Number | 延迟长度。该参数的单位为秒，取值范围为 0 ~ 1，默认值为 0.0。 |

美声变声功能的实现步骤：

1. 注册并监听 `audio-effect-enabled` 事件，在此事件触发后才能设置具体音效参数。
2. 调用 [`enableAudioEffect`](https://doc.yunxin.163.com/nertc/references/web/typedoc/Latest/zh/html/interfaces/stream.stream-1.html#enableaudioeffect) 方法开启美声和变声功能，注意：功能开启后默认无任何音效，需要手动设置。
3. 在 `audio-effect-enabled` 事件回调中调用 [`setAudioEffect`](https://doc.yunxin.163.com/nertc/references/web/typedoc/Latest/zh/html/interfaces/stream.stream-1.html#setaudioeffect) 方法设置具体的美声和变声音效。

### 示例代码

以下提供两种实现方式的示例代码：

:::::: div linked-codes
::: code 使用 CDN 插件

```JavaScript
// 创建本端 stream 实例
rtc.localStream = NERTC.createStream({
    uid: uid,                      // 本端的 uid
    audio: true,                   // 是否从麦克风采集音频
    microphoneId: microphoneId,    // 麦克风设备 deviceId，通过 getMicrophones() 获取
    video: true,                   // 是否从摄像头采集视频
    cameraId: cameraId             // 摄像头设备 deviceId，通过 getCameras() 获取
});
// 重要：必须先初始化并发布本地流，再注册插件
await rtc.localStream.init()
await rtc.localStream.publish()

const pluginOptions = {
    key: 'AIAudioEffects', // 插件名
    pluginUrl: '插件JS文件的CDN地址', // 插件 js 地址
    wasmUrl: 'wasm文件的CDN地址', // 插件依赖的 wasm 文件地址
}

// 注册事件处理函数（必须在registerPlugin调用前完成注册）
// 注册 plugin-load 事件，当插件初始化完成后回调 onPluginLoaded
rtc.localStream.on('plugin-load', onPluginLoaded);

// 插件注册失败时触发
rtc.localStream.on('plugin-load-error', (event) => {
    console.error('插件加载失败:', event.msg);
});

rtc.localStream.on('ai-denoise-enabled', () => {
  console.warn('AI 降噪已开启');
  rtc.localStream.setVoiceGate(15) //设置背景人声消除的阈值（非必须，可以不使用）
});

// 美声变声功能启用成功后触发
rtc.localStream.on('audio-effect-enabled', () => {
  console.warn('AI 音效已开启');
  // 设置音效（必须在此回调中设置，否则无效）
  const reverb = {
    wetGain: 0,
    dryGain: 1,
    damping: 1,
    roomSize: 0.1,
    decayTime: 0.1,
    preDelay: 0,
  };
  // 重要：setAudioEffect() 必须在触发 'audio-effect-enabled' 事件后调用，否则不生效
  rtc.localStream.setAudioEffect('Reverb', reverb);
  
  // 如果同时需要开启AI降噪，必须在此回调中调用
  rtc.localStream.enableAIDenoise({ 
    howlingSuppressionEnable: true //开启啸叫消除功能的配置（非必须，默认是关闭）
  });
});

// 注册 AI 音效插件
// 注意，自 v5.6.41 版本开始，如果使用 await 方式调用，事件绑定必须先完成
rtc.localStream.registerPlugin(pluginOptions);

// 'plugin-load'事件回调方法
function onPluginLoaded(name) {
  if (name === 'AIAudioEffects') {
    // 插件加载成功后，可以开启美声变声功能
    rtc.localStream.enableAudioEffect();
  }
}

// 关闭 AI 降噪（在不需要时调用）
// rtc.localStream.disableAIDenoise();

// 关闭美声变声（在不需要时调用）
// rtc.localStream.disableAudioEffect();

// 销毁插件（退出页面或完全不需要插件时调用）
// rtc.localStream.unregisterPlugin('AIAudioEffects');
```

:::
::: code 使用 NPM 插件

```JavaScript
import NERTC from 'nertc-web-sdk';
import AIAudioEffects from 'nertc-web-sdk/NERTC_Web_SDK_AIAudioEffects'

// 1. 创建本端 stream 实例
rtc.localStream = NERTC.createStream({
    uid: uid,                      // 本端的 uid
    audio: true,                   // 是否从麦克风采集音频
    microphoneId: microphoneId,    // 麦克风设备 deviceId，通过 NERTC.getMicrophones() 获取
    video: true,                   // 是否从摄像头采集视频
    cameraId: cameraId             // 摄像头设备 deviceId，通过 NERTC.getCameras() 获取
});

// 2. 初始化并发布本地流（必须在注册插件前完成）
await rtc.localStream.init();
await rtc.localStream.publish();

// 3. 定义插件配置
const pluginOptions = {
    key: 'AIAudioEffects', // 插件名
    pluginObj: AIAudioEffects, // AI 音效对象
    wasmUrl: 'https://your-domain.com/path/to/aieffects.wasm', // 插件依赖的 wasm 文件地址，请替换为实际地址
};

// 4. 在注册插件前先绑定事件监听
// 注册 plugin-load 事件，当插件初始化完成后回调 onPluginLoaded
rtc.localStream.on('plugin-load', onPluginLoaded);

// 插件注册失败时触发，event 包含错误信息，结构：{key: 插件名,msg: 详细信息}
rtc.localStream.on('plugin-load-error', (event) => {
    console.error(`插件加载失败: ${event.key}，错误信息: ${event.msg}`);
});

rtc.localStream.on('ai-denoise-enabled', () => {
  console.warn('AI 降噪已开启');
  rtc.localStream.setVoiceGate(15) //设置背景人声消除的阈值（非必须，可以不使用）
});

// 监听音效启用事件
rtc.localStream.on('audio-effect-enabled', () => {
    console.log('AI 音效已成功开启');
    
    // 设置音效参数，如混响效果
    const reverb = {
        wetGain: 0,     // 湿增益，范围 0-1
        dryGain: 1,     // 干增益，范围 0-1
        damping: 1,     // 阻尼，范围 0-1
        roomSize: 0.1,  // 房间大小，范围 0-1
        decayTime: 0.1, // 衰减时间，范围 0-1
        preDelay: 0,    // 预延迟，单位ms
    };
    
    // 注意：setAudioEffect() 必须在触发 'audio-effect-enabled' 事件后调用，否则不生效
    rtc.localStream.setAudioEffect('Reverb', reverb);
    
    // AI 降噪和美声变声同时使用时，必须在'audio-effect-enabled'回调中调用 enableAIDenoise()
    rtc.localStream.enableAIDenoise({ 
      howlingSuppressionEnable: true //开启啸叫消除功能的配置（非必须，默认是关闭）
    });
});

// 5. 注册 AI 音效插件
// 注意：自 v5.6.41 版本开始，如果使用 await 方式注册，事件绑定必须在 registerPlugin() 调用之前完成
rtc.localStream.registerPlugin(pluginOptions);

// 6. 插件加载成功回调函数
function onPluginLoaded(name) {
    if (name === 'AIAudioEffects') {
        console.log('AI 音效插件加载成功');
        
        // 在插件加载成功后启用音效功能
        // 注意：如果需要同时使用 AI 降噪，不要在此处调用 enableAIDenoise()
        // 而是在 'audio-effect-enabled' 事件回调中调用
        rtc.localStream.enableAudioEffect();
    }
}

// 7. 功能关闭示例（根据实际需要调用）
// 关闭 AI 降噪
// rtc.localStream.disableAIDenoise();

// 关闭美声变声
// rtc.localStream.disableAudioEffect();

// 8. 销毁插件（在不需要使用时调用）
// 注意：销毁后如需再次使用，需要重新注册插件
// rtc.localStream.unregisterPlugin('AIAudioEffects'); // 使用正确的插件key
```
:::
::::::

## 远端每路流美声变声

### 注意事项

- 使用远端流美声变声之前，必须确保您已经成功注册了 AI 音效插件。
- 远端流涉及到对 `stream` 的订阅/取消订阅，播放/停止播放等操作，这些操作会影响美声变声功能的状态。您需要在业务层处理这些状态变化时，正确管理美声变声功能的开启和关闭。具体操作请参考下方示例代码。
- 出于性能考虑，本端流和远端流的美声变声总数建议不要超过 6 路，超过可能会影响音频处理性能。
- 为了提升远端流变声性能，自 NERTC Web SDK 5.8.0 版本开始，支持远端音频流混流后变声功能。使用方法请参考下文 [远端流混流后变声](#setAudioEffect)。

<a id="remoteStreamSample"></a>

### 示例代码

```JavaScript
// AI 音效插件配置，需要在 remoteStream.play() 完成后才能注册插件
const pluginOptions = {
    key: 'AIAudioEffects', // 插件名
    pluginUrl: '插件JS文件地址', // 插件 js 地址
    wasmUrl: 'wasm文件地址', // 插件依赖的 wasm 文件地址
}

// 使用标志位记录美声变声状态
// 在主动开启美声变声时，设置标志位
$('#enableAudioEffect').on('click', () => {
  remoteStream.enableAudioEffect();
  remoteStream.audioEffectEnabled = true; // 记录状态
});

// 主动关闭美声变声时，更新标志位
$('#disableAudioEffect').on('click', () => {
  remoteStream.disableAudioEffect();
  remoteStream.audioEffectEnabled = false; // 更新状态
});

// 处理远端流移除事件
rtc.client.on('stream-removed', (evt) => {
  var remoteStream = evt.stream;
  console.warn('远端流停止发布: ', remoteStream.streamID, 'mediaType: ', evt.mediaType);

  // 如果音频流被移除且之前开启了美声变声，需要关闭美声变声
  if (remoteStream.audioEffectEnabled && evt.mediaType == 'audio') {
    remoteStream.disableAudioEffect();
  }
  remoteStream.stop(evt.mediaType);
});

// 处理远端流订阅成功事件
rtc.client.on('stream-subscribed', async (evt) => {
  var remoteStream = evt.stream;
  console.warn('订阅远端流成功: ', remoteStream.streamID, 'mediaType: ', evt.mediaType);

  // 播放远端流
  await remoteStream.play(remoteDiv, playOptions);

  console.log('播放远端流成功', playOptions);
  
  // 如果是音频流，注册并初始化AI音效插件
  if (evt.mediaType == 'audio') {
    const stream = remoteStream;

    // 注册事件监听（必须在registerPlugin前完成）
    stream.on('audio-effect-enabled', () => {
      // 设置默认变声效果，例如成熟音效
      stream.setAudioEffect(0, 4);
    });
    
    stream.on('plugin-load', function () {
      // 插件注册成功后开启美声变声
      stream.enableAudioEffect();
    });
    
    stream.on('plugin-load-error', (e) => {
      console.error('插件加载失败', e);
    });

    // 注册 AI 音效插件
    stream.registerPlugin(pluginOptions);
  }

  // 如果是之前已启用美声变声的流重新订阅，需要重新开启美声变声
  if (remoteStream.audioEffectEnabled && evt.mediaType == 'audio') {
    remoteStream.enableAudioEffect();
  }
});

// 取消订阅音频流时处理美声变声状态
function unsubscribeAudio(remoteStream) {
  const unsubConf = { audio: true };
  rtc.client
    .unsubscribe(remoteStream, unsubConf)
    .then(() => {
      console.log('取消订阅音频成功');
      // 如果之前开启了美声变声，需要关闭
      if (remoteStream.audioEffectEnabled) {
        remoteStream.disableAudioEffect();
      }
    })
    .catch((err) => {
      console.error('取消订阅音频失败: ', err);
    });
}

// 取消订阅整个远端流时处理美声变声状态
function unsubscribeAll(remoteStream) {
  rtc.client.unsubscribe(remoteStream).then(() => {
    // 如果开启了美声变声，需要关闭
    if (remoteStream.audioEffectEnabled && remoteStream.audio) {
      remoteStream.disableAudioEffect();
    }
  });
}

// 主动播放远端音频流时，恢复美声变声状态
async function playRemoteAudio(remoteStream, remoteDiv) {
  // 播放音频
  await remoteStream.play(remoteDiv, { audio: true });
  
  // 如果之前启用了美声变声，需要重新开启
  if (remoteStream.audio && remoteStream.audioEffectEnabled) {
    remoteStream.enableAudioEffect();
  }
}

// 主动停止播放远端音频时，处理美声变声状态
function stopRemoteAudio(remoteStream) {
  remoteStream.stop('audio');
  
  // 如果启用了美声变声，需要关闭
  if (remoteStream.audioEffectEnabled) {
    remoteStream.disableAudioEffect();
  }
}
```

<a id="setAudioEffectLite"></a>

## 远端流混流后变声

为了提升远端流变声性能，自 NERTC Web SDK 5.8.0 版本开始，支持远端音频流混流后变声功能。该功能允许开发者对远端音频进行高效的变声处理，无论是在线会议、直播还是娱乐应用，都能提供更优质的音频体验。

### **功能特点**

- **双模式变声**：提供插件变声和轻量版变声两种方案。
    :::note note
    这两种方案不可同时使用，请根据实际需求选择一种。
    :::
- **高效处理**：插件变声仅需注册一次，即可应用于所有需要变声的远端流，大大提高处理效率。
- **灵活控制**：支持部分远端流变声而其他保持原声，并且可以随时动态切换变声状态。
- **简单接入**：通过 [`play()`](https://doc.yunxin.163.com/nertc/references/web/typedoc/Latest/zh/html/interfaces/stream.stream-1.html#play) 接口的 `mixAudioStream` 参数便可控制是否参与变声混流，使用非常简便。

### **示例代码**

1. 创建 NERTC 客户端实例 `client`。
    ```JavaScript
    const client = NERTC.createClient({
        appkey: 'your_netease_rtc_appkey',
        debug: true  // 开发阶段建议开启调试
    })
    ```

2. 开启变声功能。变声功能提供两种实现方式，您可以根据需求选择其中一种。

    :::::: div linked-codes
    ::: code 插件变声法
    使用插件变声，功能更丰富，支持多种音效（需提前准备好插件文件）。
    ```JavaScript
    const pluginOptions = {
        key: 'AIAudioEffects', // 插件名
        pluginUrl: '插件JS文件地址', // 需替换为实际 URL
        wasmUrl: 'wasm文件地址', // 需替换为实际 URL
    }

    client.on('plugin-load', (name) => {
        if (name === 'AIAudioEffects') {
          // 5.8.0 版本使用以下方式
          client.adapterRef.audioMixer.on('audio-effect-enabled', () => {
              // 设置成熟音效，参数(0,4)表示使用成熟变声效果
              client.setAudioEffect(0, 4)
          })
          // 5.8.15 及之后的版本使用以下方式
          client.on('audio-effect-enabled', () => {
              // 设置成熟音效，参数(0,4)表示使用成熟变声效果
              client.setAudioEffect(0, 4)
          })

          client.enableAudioEffect()
        }
    })

    // 注册插件（确保在此之前已绑定事件监听）
    client.registerPlugin(pluginOptions)
    ```
    :::
    ::: code 轻量版变声法
    轻量版变声，资源占用更少，适合简单场景（与插件变声不可同时使用）。
    ```JavaScript
    // pitchValue: 0.5-2.0，1.0为正常音调，小于1变低沉，大于1变尖锐
    client.setAudioEffectLite('Pitch', pitchValue)
    ```
    :::
    ::::::

3. 订阅远端流并开启变声混流播放。
    ```JavaScript
    // 监听远端流事件
    client.on('stream-added', event => {
        const remoteStream = event.stream;
        // 自动订阅远端流
        client.subscribe(remoteStream).then(() => {
            console.warn(`成功订阅远端流 ${remoteStream.streamID}`);
        });
    });
    
    client.on('stream-subscribed', event => {
        // 远端流订阅成功
        const remoteStream = event.stream;
        console.warn('订阅远端流成功: ', remoteStream.streamID, 'mediaType: ', event.mediaType);
        
        // 设置远端视频画布尺寸
        remoteStream.setRemoteRenderMode({
            width: 640,
            height: 480
        });
        
        // 重要：开启混音后播放音频流，mixAudioStream:true 表示该流参与变声混流
        remoteStream.play('remoteVideoContent', {
            video: true,
            audio: true,
            mixAudioStream: true // 设置为true表示参与变声混流
        });
    });
    ```

4. 动态切换变声状态，将指定流从变声切换为非变声。

    ```JavaScript
    // remoteStream 为已经参加混流变声的远端流
    // 重要：必须先等 stop()执行完毕再调用 play()，确保资源正确释放
    await remoteStream.stop();
    
    // 重新播放但不参与变声混流
    remoteStream.play('remoteVideoContent', {
        video: true,
        audio: true,
        mixAudioStream: false // 设置为false或不传此参数，都表示不参与变声混流
    });
    ```

## 业务场景

### 场景一：本端流降噪 + 远端流混流后变声

如果您希望实现自己开启降噪功能，同时对接收到的远端用户声音应用变声效果，可以使用以下代码：

```JavaScript
// **步骤 1** 初始化本地流并注册 AI 音效插件
import NERTC from 'nertc-web-sdk';
import AIAudioEffects from 'nertc-web-sdk/NERTC_Web_SDK_AIAudioEffects';

// 创建本地流
const localStream = NERTC.createStream({
    uid: yourUid,
    audio: true,
    microphoneId: yourMicrophoneId,
    video: true,
    cameraId: yourCameraId
});

// 初始化本地流并发布
await localStream.init();
// 先发布流，后注册插件，如果考虑插件加载耗时，可使用link标签预加载插件wasm文件
await localStream.publish();

// 注册事件监听（必须在registerPlugin前完成）
localStream.on('plugin-load', (name) => {
    if (name === 'AIAudioEffects') {
        // 本地流仅开启降噪
        localStream.enableAIDenoise();
        console.log('本地流降噪功能已开启');
    }
});

localStream.on('plugin-load-error', (event) => {
    console.error('插件加载失败:', event.msg);
});

// 注册AI音效插件
localStream.registerPlugin({
    key: 'AIAudioEffects',
    pluginObj: AIAudioEffects,
    wasmUrl: '您的wasm文件地址'
});

// **步骤 2** 为client注册事件监听
client.on('plugin-load', (name) => {
    if (name === 'AIAudioEffects') {
        console.log('远端流AI插件加载成功');
        // 开启变声效果
        client.enableAudioEffect();
    }
});

client.on('audio-effect-enabled', () => {
    console.log('远端流变声效果已启用');
    // 设置成熟声音效果
    client.setAudioEffect(0, 4);
});

// 为client注册AI音效插件
client.registerPlugin({
    key: 'AIAudioEffects',
    pluginObj: AIAudioEffects,
    wasmUrl: '您的wasm文件地址'
});

// **步骤 3** 处理远端流
client.on('stream-subscribed', async (evt) => {
    const remoteStream = evt.stream;
    console.log(`已订阅远端流: ${remoteStream.streamID}`);

    // 播放远端流并启用混流变声
    await remoteStream.play('remote-stream-container', {
        audio: true,
        video: true,
        mixAudioStream: true // 启用混流变声
    });
});

// 处理流移除事件
client.on('stream-removed', (evt) => {
    const remoteStream = evt.stream;
    remoteStream.stop(evt.mediaType);
});
```

### 场景二：本端流降噪 + 本端流变声

如果您希望同时对本地音频应用降噪和变声效果，可以使用以下代码：

```JavaScript
// 初始化本地流并应用降噪和变声效果
import NERTC from 'nertc-web-sdk';
import AIAudioEffects from 'nertc-web-sdk/NERTC_Web_SDK_AIAudioEffects';

// 创建本地流
const localStream = NERTC.createStream({
    uid: yourUid,
    audio: true,
    microphoneId: yourMicrophoneId,
    video: true,
    cameraId: yourCameraId
});

// 初始化本地流并发布
await localStream.init();
// 先发布流，后注册插件，如果考虑插件加载耗时，可使用link标签预加载插件wasm文件

await localStream.publish();

// 注册事件监听
localStream.on('plugin-load', (name) => {
    if (name === 'AIAudioEffects') {
        // 开启美声变声效果
        localStream.enableAudioEffect();
    }
});

localStream.on('audio-effect-enabled', () => {
    console.log('本地流美声变声效果已启用');
    // 设置萝莉音效果
    localStream.setAudioEffect(0, 7); // 男变萝莉
    
    // 重要：如需同时开启变声和降噪，必须在audio-effect-enabled事件回调中调用enableAIDenoise
    localStream.enableAIDenoise();
    console.log('本地流降噪功能已开启');
});

localStream.on('plugin-load-error', (event) => {
    console.error('插件加载失败:', event.msg);
});

// 注册AI音效插件
localStream.registerPlugin({
    key: 'AIAudioEffects',
    pluginObj: AIAudioEffects,
    wasmUrl: '您的wasm文件地址'
});
```