- 产品相关
    - <a href="#云信音视频支持哪些平台及对应平台版本">云信支持哪些平台及对应平台版本？</a>
    - <a href="#实时音视频2.0有哪些改变">实时音视频2.0有哪些改变？</a>
    - <a href="#实时音视频2.0支持哪些场景">实时音视频2.0支持哪些场景？</a>
    - <a href="#Web端支持哪些浏览器类型和版本？">Web端支持哪些浏览器类型和版本？</a>
    - <a href="#云信支持哪些平台及对应平台版本">云信支持哪些平台及对应平台版本？</a>
    - <a href="#Web端在H5平台有哪些接口限制">Web端在H5平台有哪些接口限制</a>
    - <a href="SDK的日志从哪里获取？">SDK的日志从哪里获取？</a>
    - <a href="#如何根据场景选择合适的视频分辨率、帧率、码率？">如何根据场景选择合适的视频分辨率、帧率、码率？</a>
    - <a href="#是否支持设置用户角色？">是否支持设置用户角色？</a>
    - <a href="#SDK支持的QoS策略是什么意思？">SDK支持的QoS策略是什么意思？</a>
    - <a href="#直播场景与通信场景有什么区别？">直播场景与通信场景有什么区别？</a>
    - <a href="#一个 RTC 房间内最多支持多少人？">一个 RTC 房间内最多支持多少人？</a>
    - <a href="#房间有效期是多久？">房间有效期是多久？</a>
    - <a href="#大小流是什么意思？如何根据场景选择？">大小流是什么意思？如何根据场景选择？</a>
- 集成相关

    - <a href="#Web端如何实现音频播放">Web端如何实现音频播放？</a>
    - <a href="#Web端如何实现视频播放">Web端如何实现视频播放？</a>
    - <a href="#如何登录并进行业务管理？">如何登录并进行业务管理？</a>
    - <a href="#如何开启服务端录制及配置录制模式">如何开启服务端录制及配置录制模式？</a>
    - <a href="#怎么获取房间内的成员信息？">怎么获取房间内的成员信息？</a>
    - <a href="#安全模式和调试模式，有什么区别？">安全模式和调试模式，有什么区别？</a>
    - <a href="#如何处理音频卡如何获取通话过程中的网络状态、音量状态等数据回调顿问题">如何获取通话过程中的网络状态、音量状态等数据回调？</a>

- 质量相关
    - <a href="#为什么会出现屏幕黑屏，声音也忽大忽小？">为什么会出现屏幕黑屏，声音也忽大忽小？</a>
    - <a href="#为什么打开摄像头失败？">为什么打开摄像头失败？</a>
    - <a href="#如何处理无声问题">如何处理无声问题？</a>
    - <a href="#如何处理回声问题">如何处理回声问题？</a>
    - <a href="#如何处理音频卡顿问题">如何处理音频卡顿问题？</a>
    - <a href="#如何处理视频模糊问题？">如何处理视频模糊问题？</a>
    - <a href="#为什么视频会出现卡顿？">为什么视频会出现卡顿？</a>
    - <a href="#为什么 Android 设备开启扬声器失败，声音还是从听筒播放？">为什么 Android 设备开启扬声器失败，声音还是从听筒播放？</a> 
    - <a href="#为什么加入房间后报错 403？">为什么加入房间后报错 403？</a> 
    - <a href="#为什么加入房间后报错 414？">为什么加入房间后报错 414？</a> 



## <span id="产品相关">产品相关</span>

### <span id="云信音视频支持哪些平台及对应平台版本">云信支持哪些平台及对应平台版本</span>

NERTC SDK 支持 Android、iOS、Windows、Web、macOS 等多平台，各平台的推荐开发环境请参考[平台支持](/docs/jcyOTA0ODM/Dc5MTQzMDY)。

### <span id="实时音视频2.0有哪些改变">实时音视频2.0有哪些改变</span>

网易云信音视频第二代实时音视频产品是网易云信新一代音视频服务，以独立音视频SDK的方式进行设计和开发，是网易在第一代技术沉淀的基础上，全面升级了音视频核心的引擎与算法核心模块，融入了5G、AI等领域的设计理念，是面向于4G-5G时代推出的高品质实时音视频服务，具有更强的新技术扩展能力。

借助音视频2.0可以更加快速接入音视频，产品更轻量。


### <span id="实时音视频2.0支持哪些场景">实时音视频2.0支持哪些场景</span>

实时音视频2.0支持场景多样，包括但不限于音乐教学、语音通话、视频社交、在线课堂、远程问诊、音乐互动课堂、主播互动直播，您也可以根据自身业务形态结合音视频通话 2.0强大的音视频能力产生更多的应用场景。

### <span id="Web端支持哪些浏览器类型和版本？">Web端支持哪些浏览器类型和版本？</span>

请参考 [音视频应用浏览器兼容情况](https://doc.yunxin.163.com/general/concept/zAwMTg4NjA?platform=others)。

### <span id="Web端在H5平台有哪些接口限制">Web端在H5平台有哪些接口限制</span>

Web 端 NERTC SDK 在 H5 平台不支持以下 API：

- 不支持以下伴音相关 API。

  startAudioMixing、stopAudioMixing、pauseAudioMixing、resumeAudioMixing、adjustAudioMixingVolume、getAudioMixingDuration。

  如果您调用了以上 API，SDK 会报错 `BROWSER_NOT_SUPPORT`。
- 不支持以下客户端录制功能相关 API：      
  
  startMediaRecording、stopMediaRecording、playMediaRecording、listMediaRecording、cleanMediaRecording、downloadMediaRecording。
  
  如果您调用了以上 API，SDK 会报错 `RecordBrowserNotSupport`。
- 不支持设置麦克风采集音量（setCaptureVolume）。

### <span id="SDK的日志从哪里获取？">SDK的日志从哪里获取</span>

各端设置日志路径的方式如下：

- Android：
    调用 `NERtc.getInstance().init` 方法初始化 SDK 时，通过 `NERtcOption` 对象的 `LogDir` 参数指定日志路径。
- iOS：
    调用 `setupEngineWithContext` 方法初始化 SDK 时，通过 `NERtcEngineContext` 类下 `NERtcLogSetting` 的 `logSetting` 参数指定日志路径。SDK 日志会保存在沙盒中该路径下的 **Document>NERtcSDK** 目录中。
- PC：
    调用 `initialize` 方法初始化 SDK 时，通过 `rtc_engine_context` 对象的 `log_dir_path` 参数指定日志路径。
- Web：
    调用 `WebRTC2.createClient` 方法创建客户端时，通过设置 `debug` 参数为 true 开启调试模式，SDK 将在浏览器控制台输出日志。




### <span id="如何根据场景选择合适的视频分辨率、帧率、码率？">如何根据场景选择合适的视频分辨率、帧率、码率？</span>

视频分辨率等参数的选择根据使用场景来决定，例如老师和学生在房间内进行1对1通话，视频布局比较大，分辨率随之高一点，帧率和码率也会提高，如果是房间内有多人，那么每个视频布局会相对比较小，所以分辨率等参数可以低一点。
为您推荐的场景参数如下所示：

2人视频通话场景：
- 分辨率 320 x 240、帧率 15 fps、码率 200 Kbps
- 分辨率 640 x 360、帧率 16 fps、码率 400 Kbps

多人视频通话场景：
- 分辨率 160 x 120、帧率 15 fps、码率 65 Kbps
- 分辨率 320 x 180、帧率 15 fps、码率 140 Kbps
- 分辨率 320 x 240、帧率 15 fps、码率 200 Kbps

### <span id="是否支持设置用户角色？">是否支持设置用户角色？</span>

NERTC SDK 支持通过接口 setClientRole 设置用户角色。

当前支持的用户角色包括主播（broadcaster）和观众（audience）。加入房间时，用户默认为主播角色，主播和观众的权限不同。

<table> 
<tr> 
<th width=20%><b>角色</b></th> 
<th width=40%><b>权限</b></th> 
</tr> 
<tr> 
<td>主播</td> 
<td><ul><li>操作摄像头等音视频设备。<li>发布音视频流<li>配置互动直播推流任务<li>上下线行为对房间内其他用户可见。</td> 
</tr> 
<tr> 
<td>观众</td> 
<td>接收音视频流。</td> 
</tr> 
</table>

### <span id="SDK支持的QoS策略是什么意思？">SDK支持的QoS策略是什么意思？</span>

QoS: Quality of Service，服务质量。

当参与音视频通话的用户网络较差时，SDK会启动QoS策略，自动调整收发数据的分辨率、码率、帧率。
多人音视频通话：A、B、C、D通话。

- 对于视频数据

    如果A上行发送网络较差，或者B、C、D下行接收网络较差，服务器都会回调给A并触发QoS，调整A发送的数据。
- 对于音频数据

    如果A上行发送网络较差，则服务器回调给A并触发QoS，调整A发送的数据；

    如果B、C、D下行接收网络较差，则服务器根据B、C、D的网络情况重新编码音频数据发送给B、C、D。

### <span id="直播场景与通信场景有什么区别？">直播场景与通信场景有什么区别？</span>

NERTC SDK 通过 setChannelProfile 方法设置实时音视频通话的场景，您可以通过该方法将房间设置为通信场景或直播场景，默认为通信场景。网易云信会针对不同实时音视频场景设置不同的优化策略，例如用户角色、默认视频编码码率等。

通信场景设置推荐用于一对一或多人音视频通话场景，直播场景设置推荐用于语音聊天室、小班课、主播PK等互动直播场景。

直播场景和通信场景的差异如下：

项 | 直播场景 | 通信场景
---- | -------------- | ---------
用户角色 | 用户默认角色为主播，可以发送音视频流、配置推流任务等。通过方法 setClientRole 可切换用户角色，切换为观众后，只能接收音视频流。 | 用户默认角色为主播。不支持切换用户角色为观众。
QoS 策略 | 在直播场景下，NERTC SDK 的 QoS 策略控制侧重于保证画质清晰度。因此在默认情况下，如果分辨率和帧率相同，直播场景的码率相较于通信场景更高。在弱网环境下会有一定延时。 |在通信场景下，NERTC SDK 的 QoS 策略控制侧重于保证音视频通话的实时性，最大程度上保证低时延。在弱网环境下会降低音质、画质来保证音视频通话流畅。|

      

### <span id="房间有效期是多久？">房间有效期是多久？</span>

调用服务端 API 创建房间成功后，若 24 小时内无人加入房间，房间会自动销毁；当所有房间成员均离开房间后，房间会立刻销毁。

::: note note
若客户端调用 [`Client.join`](https://doc.yunxin.163.com/nertc/api-refer/web/typedoc/Latest/zh/html/interfaces/client.client-1.html#join) 方法后未成功加入房间，服务端可能也会创建一个新的音视频房间。
:::

### <span id="一个 RTC 房间内最多支持多少人？">一个 RTC 房间内最多支持多少人？</span>

NERTC SDK V 4.1.0 版本开始支持多至上万人的音视频通话，最多支持 60 人视频通话，可用于线上年会、远程会议、学术论坛等需要多人实时音视频通话的场景。在 V 4.1.0 版本中，NERTC SDK 针对 200 人以上的音视频通话场景进行了技术优化与改进，提高了多人通话的连通性与语音效果，在大房间音视频通话场景中为您提供更优质的语音视频体验。

<note type="note">如果您的业务场景中存在 200 人以上的大房间需求，请将 NERTC SDK 升级至 V4.1.0 及后续版本。</note>

### <span id="大小流是什么意思？如何根据场景选择？">大小流是什么意思？如何根据场景选择？</span>
大流对应高清画质，小流对应低清画质。多人音视频通话过程中，为了减少下行带宽占用，可以开启大小流模式，每个用户会上传一大一小两个视频流，接收方可以根据显示需要来选择接收大流或是小流。

## <span id="集成相关">集成相关</span>


### <span id="Web端如何实现音频播放">Web端如何实现音频播放</span>
云信SDK为您提供相关接口，实现音频播放功能，接口如下：
- 播放远端声音
  ``` localStream.play()
  //remoteStream.play() 
  ```
- 停止播放远端声音
  ```
  localStream.stop()
  //remoteStream.stop()
  ```
由于浏览器策略，可能会限制页面声音的自动播放，本文以chrome浏览器为您示例。
> SDK优化过音频播放逻辑，一般是用户再有页面刷新的业务场景时，才可能出现音频播放失败的问题，因为此时用户还没有交互。

自动播放会报错：
chrome浏览器不允许声音自动播放，如果用显式调用play进行播放会出现如下报错：``DOMException: play() failed because the user didn't interact with the document first.（用户还没有交互，不能调用play）。
``

相关解决方法：
- 您可以手动修改浏览器的配置，解决该问题。

  1. 打开chrome浏览器。

  2. 输入``chrome://flags/#autoplay-policy``。

  3. 在配置页面找到``Autoplay policy``，将``Default``修改为``No user gesture is required``。

  4. 重启浏览器使配置生效。

- 用户还没有交互，音频不能自动播放，用户的交互包括用户触发的touchend、click、doubleclick或者keydown事件，在这些事件里面音频就能够正常播放了。

  因此您可以在Web页面上加一个类似静音的标志按钮，在用户点击按钮时，调用SDK的音频播放接口。

### <span id="Web端如何实现视频播放">Web端如何实现视频播放</span>
云信SDK为您提供相关接口，实现音频播放功能，接口如下：
- 预览本地或者远端摄像头（调用该方法进行本地、远端预览）
  ```
  let div = document.getElementById('local-container')
  localStream.play(div)

  //let div = document.getElementById('remote-container')
  //remoteStream.play(div)
  ```

- 停止预览本地或者远端摄像头（调用该方法关闭本地、远端预览）
  ```
  localStream.stop()
  //remoteStream.stop()
  ```
- 设置本地视频画面大小（开启预览本地摄像头捕获的视频流后，可以通过该方法动态调节预览画面的大小）
  ```
  let config = {
  width: 160,  //窗口宽
  height: 120, //窗口高
  cut: true //是否允许裁剪 
  }
  localStream.setLocalRenderMode(config)
  ```


  参数说明：


  | param参数属性 | 类型 | 说明             | param参数属性 |
  | :---------- | :---------- | :---------- | :---------- |
  | width         | number | 需要预览画面的宽度 | width         |
  | height        | number | 需要预览画面的高度 | height        |
  | cut           | bool   | 是否进行裁剪 | cut           |




cut设置为false时，设置的预览大小将按照捕获的原始视频宽高比进行等比缩放，会出现某一个方向撑不满容器，导致黑边的情况。

cut设置为true时，默认将原始画面按照1:1进行裁剪。




- 设置远程视频画面大小（收到远程流并启动预览之后，可以通过该方法动态调节预览画面的大小，用法和上面的设置本地视频画面大小一样）。

  ```
  let config = {
  width: 160,  //窗口宽
  height: 120, //窗口高
  cut: true //是否允许裁剪 
  }
  remoteStream.setRemoteRenderMode(config)
  ```

  参数说明：



  | param参数属性 | 类型 | 说明             | param参数属性 |
  | :---------- | :---------- | :---------- | :---------- |
  | width         | number | 需要预览画面的宽度 | width         |
  | height        | number | 需要预览画面的高度 | height        |
  | cut           | bool   | 是否进行裁剪 | cut           |



cut设置为false时，设置的预览大小将按照捕获的原始视频宽高比进行等比缩放，会出现某一个方向撑不满容器，导致黑边的情况。cut设置为true时，默认将原始画面按照1:1进行裁剪。



### <span id="如何登录并进行业务管理？">如何登录并进行业务管理？</span>

进入[**云信官网**](https://yunxin.163.com/)，点击右上角的**登录控制台**，输入账号与密码即可完成登录。

在对应的云信应用的**功能管理**一栏中可以看到当前已开通的功能，找到**音视频通话G2**，点击**功能管理**即可进行相关业务的配置。

当前支持录制配置，具体包括：

- 录制画布设置。

- 录制布局模式。

- 音视频录制自定义布局配置。

### <span id="如何开启服务端录制及配置录制模式">如何开启服务端录制及配置录制模式</span>

首先需要在<a href="https://yunxin.163.com/">官网首页</a>通过微信、在线消息或电话等方式联系云信商务经理开通服务端录制功能。

V4.5.0及之后版本，云端录制只需要在服务端进行相关配置，客户端无需再单独配置，服务端的配置方法请参见[新版云端录制方案](/docs/jcyOTA0ODM/DI2OTE0ODI?platformId=50326)。

### <span id="怎么获取房间内的成员信息？">怎么获取房间内的成员信息？</span>
- 您可以通过调用服务端API进行查询，接口：```https://logic-dev.yunxinapi.com/v2/api/rooms/{cid}/members HTTP/1.1```。
- 通过客户端SDK回调维护。

加入房间后，新加入的成员会收到「用户加入房间通知」，返回已经在房间内的其他成员的账号信息；已经在房间内的其他成员也会通过该通知，收到当前新加入的成员的账号信息。

例如，某个音视频房间已有A、B、C、D，当E加入时，E会收到A/B/C/D加入房间的通知，A/B/C/D也会收到E加入房间的通知。

### <span id="安全模式和调试模式，有什么区别？">安全模式和调试模式，有什么区别？</span>

安全模式：客户端需要AppKey和token来完成认证进行实时通话。 其中token需要第三方服务器从云信服务器获取。

调试模式：客户端只需要AppKey即可完成认证进行实时通话，这种情况下用户需要保管好AppKey，防止泄露，默认情况下调试模式处于关闭状态，如需开启，请在控制台中将指定应用的鉴权方式设置为调试模式。

### <span id="如何获取通话过程中的网络状态、音量状态等数据回调顿问题">如何获取通话过程中的网络状态、音量状态等数据回调</span>

云信音视频 SDK 在通话过程中会回调通话中的各种状态信息，包括本地音频流统计信息、本地视频流统计信息、通话中远端音频流的统计信息、通话中远端视频流的统计信息等。
<br>Web 端的实现方式请参考如下代码：

  ```
    setInterval(async () => {
    const localAudioStats = await rtc.client.getLocalAudioStats();
    if (localAudioStats[0]){
        console.log(`===== localAudioStats =====`);
        console.log(`Audio CodecType: ${localAudioStats[0].CodecType}`);
        console.log(`Audio MuteState: ${localAudioStats[0].MuteState}`);
        console.log(`Audio RecordingLevel: ${localAudioStats[0].RecordingLevel}`);
        console.log(`Audio SamplingRate: ${localAudioStats[0].SamplingRate}`);
        console.log(`Audio SendBitrate: ${localAudioStats[0].SendBitrate}`);
        console.log(`Audio SendLevel: ${localAudioStats[0].SendLevel}`);
    }
    }, 1000)

    setInterval(async () => {
    const remoteAudioStatsMap = await rtc.client.getRemoteAudioStats();
    for(var uid in remoteAudioStatsMap){
        console.log(`Audio CodecType from ${uid}: ${remoteAudioStatsMap[uid].CodecType}`);
        console.log(`Audio End2EndDelay from ${uid}: ${remoteAudioStatsMap[uid].End2EndDelay}`);
        console.log(`Audio MuteState from ${uid}: ${remoteAudioStatsMap[uid].MuteState}`);
        console.log(`Audio PacketLossRate from ${uid}: ${remoteAudioStatsMap[uid].PacketLossRate}`);
        console.log(`Audio RecvBitrate from ${uid}: ${remoteAudioStatsMap[uid].RecvBitrate}`);
        console.log(`Audio RecvLevel from ${uid}: ${remoteAudioStatsMap[uid].RecvLevel}`);
        console.log(`Audio TotalFreezeTime from ${uid}: ${remoteAudioStatsMap[uid].TotalFreezeTime}`);
        console.log(`Audio TotalPlayDuration from ${uid}: ${remoteAudioStatsMap[uid].TotalPlayDuration}`);
        console.log(`Audio TransportDelay from ${uid}: ${remoteAudioStatsMap[uid].TransportDelay}`);
    }
    }, 1000)

    setInterval(async () => {
    const localVideoStats = await rtc.client.getLocalVideoStats();
    for (var i in localVideoStats){
        let mediaType = (i === 0 ? "video" : "screen")
        console.log(`===== localVideoStats ${mediaType} =====`);
        console.log(`${mediaType} CaptureFrameRate: ${localVideoStats[i].CaptureFrameRate}`);
        console.log(`${mediaType} CaptureResolutionHeight: ${localVideoStats[i].CaptureResolutionHeight}`);
        console.log(`${mediaType} CaptureResolutionWidth: ${localVideoStats[i].CaptureResolutionWidth}`);
        console.log(`${mediaType} EncodeDelay: ${localVideoStats[i].EncodeDelay}`);
        console.log(`${mediaType} MuteState: ${localVideoStats[i].MuteState}`);
        console.log(`${mediaType} SendBitrate: ${localVideoStats[i].SendBitrate}`);
        console.log(`${mediaType} SendFrameRate: ${localVideoStats[i].SendFrameRate}`);
        console.log(`${mediaType} SendResolutionHeight: ${localVideoStats[i].SendResolutionHeight}`);
        console.log(`${mediaType} SendResolutionWidth: ${localVideoStats[i].SendResolutionWidth}`);
        console.log(`${mediaType} TargetSendBitrate: ${localVideoStats[i].TargetSendBitrate}`);
        console.log(`${mediaType} TotalDuration: ${localVideoStats[i].TotalDuration}`);
        console.log(`${mediaType} TotalFreezeTime: ${localVideoStats[i].TotalFreezeTime}`);
    }
    }, 1000)

    setInterval(async () => {
    const remoteVideoStatsMap = await rtc.client.getRemoteVideoStats();
    for(var uid in remoteVideoStatsMap){
        console.log(`Video End2EndDelay from ${uid}: ${remoteVideoStatsMap[uid].End2EndDelay}`);
        console.log(`Video MuteState from ${uid}: ${remoteVideoStatsMap[uid].MuteState}`);
        console.log(`Video PacketLossRate from ${uid}: ${remoteVideoStatsMap[uid].PacketLossRate}`);
        console.log(`Video RecvBitrate from ${uid}: ${remoteVideoStatsMap[uid].RecvBitrate}`);
        console.log(`Video RecvResolutionHeight from ${uid}: ${remoteVideoStatsMap[uid].RecvResolutionHeight}`);
        console.log(`Video RecvResolutionWidth from ${uid}: ${remoteVideoStatsMap[uid].RecvResolutionWidth}`);
        console.log(`Video RenderFrameRate from ${uid}: ${remoteVideoStatsMap[uid].RenderFrameRate}`);
        console.log(`Video RenderResolutionHeight from ${uid}: ${remoteVideoStatsMap[uid].RenderResolutionHeight}`);
        console.log(`Video RenderResolutionWidth from ${uid}: ${remoteVideoStatsMap[uid].RenderResolutionWidth}`);
        console.log(`Video TotalFreezeTime from ${uid}: ${remoteVideoStatsMap[uid].TotalFreezeTime}`);
        console.log(`Video TotalPlayDuration from ${uid}: ${remoteVideoStatsMap[uid].TotalPlayDuration}`);
        console.log(`Video TransportDelay from ${uid}: ${remoteVideoStatsMap[uid].TransportDelay}`);
    }
    }, 1000)
    
  ```


## <span id="质量相关">质量相关</span>

### <span id="为什么会出现屏幕黑屏，声音也忽大忽小？">为什么会出现屏幕黑屏，声音也忽大忽小？</span>
Android和iOS SDK在语音通话/音频模式时，默认会开启距离传感器。
可能出现以下情况：    
    - 当检测到移动设备靠近人脸时，可能出现：自动关闭屏幕（黑屏），并且将声音输出从扬声器切换到听筒(因此声音变小)。
    - 当检测到移动设备远离人脸时，可能出现：重新开启屏幕（亮起），并且将声音输出从听筒切换回扬声器(因此声音变大)。
    - 如果反复靠近或者远离，则可能出现屏幕黑屏又亮起，声音忽大忽小的情况。

### <span id="为什么打开摄像头失败？">为什么打开摄像头失败？</span>

摄像头打开失败有多种原因，您可以参考如下步骤进行排查：

1. 确认摄像头权限有没有打开。Android、iOS/macOS系统都有权限管理，请在系统设置中检查。同时 Android 上有些安全软件也管理权限。

2. 检查是否有其他应用占据了摄像头。关闭其他应用，重启手机再试。

3. 摄像头硬件问题。打开系统自带的拍摄视频程序看是否可以录像。

### <span id="如何处理无声问题">如何处理无声问题</span>

无声问题定义：通话过程中或通话全程，单方或多方听不到声音。

问题处理思路：如下图所示，发送端与接收端之间有多个音频处理与传输过程，建议通过采集、编码、网络传输、解码、播放等各模块对问题进行定位。
- 检查采集及播放设备的使用权限。
- 检查音视频房间的连接状态。
- 检查发布/订阅接口调用。
- 检查扬声器/麦克风打开状态。
- 检查静音接口调用。
- 检查外接设备状态，如蓝牙耳机、音箱；比如设备使用的是 Windows 系统，需检查驱动程序或使用系统自带的设备自检。

如以上内容均未存在问题，请联系技术支持，云信技术支持会及时提供技术服务帮助问题定位及解决。

![音频处理流程](https://yx-web-nosdn.netease.im/quickhtml%2Fassets%2Fyunxin%2Fdoc%2FG2-FAQ-soundless.png)

### <span id="如何处理回声问题">如何处理回声问题</span>

回声问题定义：本端在音频传入时，扬声器播放自己传入的音频。

问题处理思路：如下图所示，发送端听到本端的声音，一般为接收端回声消除处理出现不可靠的情况导致，如在多人的场景下，需先定位明确的异常接收端。

- 隔离通话双方设备的物理距离。
- 依次静音房间内的用户，判断出现回声的异常源。
- 异常源佩戴耳机的情况下可去除回声。
- 提供异常源的设备具体信息包含sdk客户端日志。

请将收集的异常源信息提供给云信技术支持，云信技术支持会及时反馈异常处理进度及结果，该过程一般无需进行 SDK 替换。

![回声处理流程](https://yx-web-nosdn.netease.im/quickhtml%2Fassets%2Fyunxin%2Fdoc%2FG2-FAQ-echo.png)

### <span id="如何处理音频卡顿问题">如何处理音频卡顿问题</span>

卡顿问题定义：本端在收听其它端的音频效果时出现一次或多次声音不连续的情况。

问题处理思路：卡顿问题一般由设备、网络两个因素条件引起，其中网络包含发送端的上行网络及接收端的下行网络。

具体的排查思路如下： 

- 检查通话用的网络情况，可切换到稳定的4G或wifi尝试。
- 通过SDK回调的设备及网络状态信息判断
- 手机通话用户的日志信息或账号信息提供给云信技术支持，云信技术支持会及时通过您的信息反馈卡顿问题定位进度及结果。

### <span id="如何处理视频模糊问题？">如何处理视频模糊问题？</span>


视频模糊一般是由视频码率或分辨率过低导致。


1. 确认 SDK 中分辨率的设置，您可以通过 setLocalVideoConfig 方法来设置视频相关的属性。

2. 尝试通过 4G 连接，或者其他 Wifi 信号排除网络问题。

3. 接收端接收的是大流还是小流，是小流的话可以调用接口申请大流并关闭小流。

4. 如果有视频前处理，请先关闭前处理进行测试，排除前处理的问题。

### <span id="为什么视频会出现卡顿？">为什么视频会出现卡顿？</span>

视频卡顿问题一般由网络、设备性能等原因造成。

- 判断是持续性的还是一次性的卡顿。一次性的卡顿是由网络和设备的随机性导致，属于正常现象。
- 检查网络状态，判断连接是否正常，是否能够上网。
- 如果网络连接正常但依然卡顿，请尝试更换网络连接，检查在网络状态良好的条件下是否依然卡顿。
- 如果网络良好且条件允许，请尝试更换设备。
- 如果有视频前处理，例如美颜等，请先关闭前处理，检查卡顿是否由于前处理导致。

### <span id="为什么 Android 设备开启扬声器失败，声音还是从听筒播放？">为什么 Android 设备开启扬声器失败，声音还是从听筒播放？</span>

  请检查 SDK 日志文件``nrtc_engine.log``中是否有记录``Permission miss : android.permission.MODIFY_AUDIO_SETTINGS``。如有，需要在 `AndroidManifest.xml` 中配置：

``<uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS"/>``

### <span id="为什么加入房间后报错 403？">为什么加入房间后报错 403？</span>

一般是由于您在使用 Demo 时，没有开通**调试模式**导致。

网易云信默认关闭**调试模式**，您在此模式下调用加入房间的接口时，需要传入对应的 Token。而当您使用 Demo 加入房间时，Token 入参为 null，只有在**调试模式**下才能成功加入房间。为了快速测试并体验产品，建议联系商务经理开通**调试模式**。

### <span id="为什么加入房间后报错 414？">为什么加入房间后报错 414？</span>

返回 414 错误码的可能原因如下：

- 在**调试模式**下传了 Token，或者在**安全模式**下没有传 Token。
- Token 计算错误。需要检查服务端获取 Token 的方式。
- App Key、Token、uid、channelName 不匹配。
- uid 传入了非 long 参数或者负数。

