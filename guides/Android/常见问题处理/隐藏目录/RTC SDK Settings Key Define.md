## **Public Keys**

### 1. channel
| Name | 定义 | 涉及的端 | 值类型及其含义 | 发布版本 |
| ---- | ---- | ---- | ---- | ---- |
| kNERtcSettingEnable1V1Mode | sdk.enable.1v1.Mode | android: key_enable_1v1_mode<br>ios: kNERtcKeyChannel1V1ModeEnabled<br>mac/windows: channel_1v1_mode_enabled | 是否开启双人通话模式。适用于 1v1 通话场景<br>默认值: false | 4.6.0 之前 |
| kNERtcSettingEnablePushSelfStream | sdk.enable.push.self.stream | android: key_publish_self_stream<br>ios: kNERtcKeyPublishSelfStreamEnabled<br>mac/windows: publish_self_stream_enabled | 在旁路推流场景中，是否允许推送本地媒体流到 CDN<br>默认值: true | 4.6.0 之前 |
| kNERtcSettingSetLogLevel | sdk.set.log.level | ios: kNERtcKeyLogLevel<br>mac/windows: log_level | 设置 SDK 日志等级<br>默认值: kNERtcLogLevelInfo | 4.6.0 之前 |
| kNERtcSettingSetExtraInfo | sdk.set.extra.info | android: key_custom_extra_info<br>ios: kNERtcKeyExtraInfo<br>mac/windows: extra_info | Login 事件中的一个自定义字段，适用于标识一些额外信息，例如 App 版本<br>默认值: 无 | 4.6.0 之前 |

### 2. record

| Name | 定义 | 涉及的端 | 值类型及其含义 | 发布版本 |
| ---- | ---- | ---- | ---- | ---- |
| kNERtcSettingSetRecordType | sdk.set.record.type | android: key_server_record_mode<br>ios: kNERtcKeyRecordType<br>macwindows: record_type | 云端录制模式<br>```{kNERtcRecordTypeAll = 0, kNERtcRecordTypeMix = 1, kNERtcRecordTypeSingle = 2}```<br>默认值: 无 | 4.6.0 之前 |
| kNERtcSettingEnableRecordHost | sdk.enable.record.host | android: key_server_record_speaker<br>ios: kNERtcKeyRecordHostEnabled<br>mac/windows: record_host_enabled | 是否云端录制主讲人<br>默认值: false | 4.6.0 之前 |
| kNERtcSettingEnableRecordAudio | sdk.enable.record.audio | android: key_server_record_audio<br>ios: kNERtcKeyRecordAudioEnabled<br>mac/windows: record_audio_enabled | 是否开启云端音频录制<br>默认值: false | 4.6.0 之前 |
| kNERtcSettingEnableRecordVideo | sdk.enable.record.video | android: key_server_record_video<br>ios: kNERtcKeyRecordVideoEnabled<br>mac/windows: record_video_enabled | 是否开启云端视频录制<br>默认值: false | 4.6.0 之前 |

### 3. video

| Name | 定义 | 涉及的端 | 值类型及其含义 | 发布版本 |
| ---- | ---- | ---- | ---- | ---- |
| kNERtcSettingPreferEncodeType | sdk.prefer.encode.mode | android: key_video_encode_mode<br>ios: kNERtcKeyVideoPreferHWEncode | 是否优先使用硬件编码视频数据<br>android/mac/windows: ```{MEDIA_CODEC_DEFAULT, MEDIA_CODEC_HARDWARE, MEDIA_CODEC_SOFTWARE},```<br>ios: ```true or false```<br>默认值: android: MEDIA_CODEC_DEFAULT<br>ios: true<br>mac/windows: MEDIA_CODEC_SOFTWARE | 4.6.0 之前 |
| kNERtcSettingPreferDecodeType | sdk.prefer.decode.mode | android: key_video_decode_mode<br>ios: kNERtcKeyVideoPreferHWDecode | 是否优先使用硬件解码视频数据<br>android/mac/windows: ```{MEDIA_CODEC_DEFAULT, MEDIA_CODEC_HARDWARE, MEDIA_CODEC_SOFTWARE},```<br>ios: ```true or false```<br>默认值: android: MEDIA_CODEC_DEFAULT<br>ios: true<br>mac/windows: MEDIA_CODEC_SOFTWARE | 4.6.0 之前 |
| kNERtcSettingAutoSubscribeVideo | sdk.auto.subscribe.video | android: key_auto_subscribe_video<br>ios: kNERtcKeyAutoSubscribeVideo<br>mac/windows: auto_subscribe_video | 是否自动订阅其他用户的视频流<br>默认值: false | 4.6.0 之前 |
| kNERtcSettingPublishVideoMode | sdk.publish.video.mode | android: key_video_send_mode<br>ios: kNERtcKeyVideoSendOnPubType<br>mac/windows: video_sendonpub_type | 是否自动订阅其他用户的视频流<br>```{kNERtcVideoSendOnPubWithNone = 0, kNERtcVideoSendOnPubWithHigh = 1, kNERtcVideoSendOnPubWithLow = 2, kNERtcVideoSendOnPubWithAll = 3}```<br>默认值: kNERtcVideoSendOnPubWithHigh | 4.6.0 之前 |
| kNERtcSettingEnableVideoCaptureObserver | sdk.enable.video.capture.observer | ios: kNERtcKeyVideoCaptureObserverEnabled<br>mac/windows: video_frame_capture | 是否需要开启视频数据采集回调<br>默认值: false | 4.6.0 之前<br>mac/windows: 4.5.901 |
| kNERtcSettingMirrorWithFrontCamera | sdk.mirror.with.front.camera | android: key_video_local_preview_mirror | 是否开启前置摄像头预览镜像<br>默认值: true | 4.6.0 之前 |
| kNERtcSettingStartVideoWithBackCamera | sdk.start.video.with.back.camera | ios: kNERtcKeyVideoStartWithBackCamera | 第一次开启摄像头时，是否使用后摄像头<br>默认值: false | 4.6.0 之前 |
| kNERtcSettingPreferMetalRender | sdk.prefer.metal.render | ios: kNERtcKeyVideoPreferMetalRender | 是否优先使用 Metal 渲染<br>默认值: false | 4.6.0 之前 |
| kNERtcSettingSetCameraType | sdk.set.camera.type | android: key_video_camera_type | 设置摄像头类型<br>```{int kLiteSDKCameraTypeUnknown = 0, int kLiteSDKCamera1 = 1, int kLiteSDKCamera2 = 2}```<br>默认值: ??? | 4.6.0 之前 |

### 4. audio

| Name | 定义 | 涉及的端 | 值类型及其含义 | 发布版本 |
| ---- | ---- | ---- | ---- | ---- |
| kNERtcSettingAutoSubscribeAudio | sdk.auto.subscribe.audio | android: key_auto_subscribe_audio<br>ios: kNERtcKeyAutoSubscribeAudio<br>mac/windows: auto_subscribe_audio | 是否自动订阅其他用户的音频流<br>默认值: true | 4.6.0 之前 |
| kNERtcSettingEnableReportVolumeWithMute | sdk.enable.report.volume.with.mute | android: key_enable_report_volume_when_mute<br>ios: KNERtcKeyEnableReportVolumeWhenMute<br>mac/windows: enable_report_volume_when_mute | 本地用户静音时是否返回原始音量<br>默认值: false | 4.6.0 之前 |
| kNERtcSettingEnableAudioAINS | sdk.enable.audio.ains | android: key_audio_ai_ns_enable<br>ios:KNERtcKeyAudioAINSEnable<br>mac/windows: audio_processing_ai_ns_enable | 是否开启 AI 降噪<br>默认值: android: true<br>其他: false | 4.6.0 之前 |
| kNERtcSettingEnableEarphone | sdk.enable.earphone | mac/windows: audio_processing_earphone | 通知 SDK 是否使用耳机<br>默认值: false | 4.6.0 之前 |
| kNERtcSettingAudioEnableBluetoothSCO | sdk.audio.enable.bluetooth.sco | android: key_audio_bluetooth_sco | true:<br>默认值: false | 4.6.0 之前 |
| kNERtcSettingSupportCallkit | sdk.support.callkit | ios | 是否需要支持 Callkit 框架<br>默认值: false | 4.0.0 之前 |
| kNERtcSettingSetAudioDeviceAutoSelectType | sdk.set.audio.device.auto.select.type | mac/windows: audio_device_auto_select_type | true:<br>默认值: false | 4.6.0 之前 |
| kNERtcSettingDisableOverrideSpeakerOnReceiver | sdk.disable.override.speaker.on.receiver | ios: KNERtcKeyDisableOverrideSpeakerOnReceiver | 当系统切换听筒或扬声器时，SDK 是否以系统设置为准<br>默认值: false | 4.6.0 之前 |
| kNERtcSettingDisableSoftwareAecOnHeadset | sdk.disable.software.aec.on.headset | ios: KNERtcKeyDisableSWAECOnHeadset | 设置耳机时不使用软件回声消除功能<br>默认值: false | 4.6.0 之前 |

### 5. data channel

| Name | 定义 | 涉及的端 | 值类型及其含义 | 发布版本 |
| ---- | ---- | ---- | ---- | ---- |
| kNERtcSettingAutoSubscribeData | sdk.auto.subscribe.data | android: key_auto_subscribe_data<br>ios: kNERtcKeyAutoSubscribeData<br>mac/windows: auto_subscribe_data | 是否自动订阅其他用户的数据通道<br>默认值: false | 5.0.0 |

## **Private Keys**

### 1. channel
| Name | 定义 | 涉及的端 | 值类型及其含义 | 发布版本 |
| ---- | ---- | ---- | ---- | ---- |
| kNERtcSettingEnableDebugEnvironment | sdk.enable.debug.environment | android: key_test_server_uri<br>ios: nertc.engine.debug.setting.enabled<br>mac/windows: test_env | 是否开启测试环境<br>默认值: false | 4.6.0 之前 |
| kNERtcSettingEnableEncryptMedia | sdk.enable.encrypt.media | android: key_data_encrypt_mode<br>ios: kNERtcKeyMediaDataEncryptEnabled<br>mac/windows:encrypt_enabled | 是否开启媒体数据传输加密<br>默认值: false | 4.6.0 之前 |
| kNERtcSettingSetTuneServer | sdk.set.forward.server | android: key_media_server_uri<br>ios: nrtc.g2.demo.server.uri<br>mac/windows: turn_server | 设置媒体服务器地址<br>默认值: 无 | 4.6.0 之前 |
| kNERtcSettingSetSignalProxyServer | sdk.set.tune.server | android: key_quic_server_uri<br>ios: ntrc.demo.app.quic.addr<br>mac/windows: signal_proxy_server | 设置 quic 服务器地址<br>默认值: 无 | 4.6.0 之前 |
| kNERtcSettingSetForwardServer | sdk.set.signal.proxy.server | android: key_dispatcher_forwarded_ip<br>ios: nrtc.g2.demo.local.ip.for.dispatch<br>mac/windows: forward_server | 设置本地 forward ip<br>默认值: 无 | 4.6.0 之前 |
| kNERtcSettingEnableEncryptLog | sdk.enable.encrypt.log | android/ios/mac/windows | 是否开启日志加密<br>默认值: true | 5.0.0 |

### 2. video

| Name | 定义 | 涉及的端 | 值类型及其含义 | 发布版本 |
| ---- | ---- | ---- | ---- | ---- |
| kNERtcSettingPreferH265 | sdk.prefer.h265 | android: key_h265_switch<br>ios: kNERtcKeyVideoPreferH265<br>mac/windows: video_enable_h265 | 是否优先使用 H265 算法编解码视频数据<br>默认值: false | 4.6.0 之前 |
| kNERtcSettingPreferNevc | sdk.prefer.nevc | android: key_nevc_switch<br>ios: kNERtcKeyVideoPreferNEVC<br>mac/windows: video_enable_nevc | 是否优先使用 Nevc 算法编解码视频数据<br>默认值: false | 4.6.0 之前 |
| kNERtcSettingSetVP8Mode | sdk.set.vp8.mode | android: key_vp8_mode<br>ios: kNERtcKeyVideoVP8Mode<br>mac/windows: video_vp8_mode | 是否优先使用 VP8 算法编解码视频数据<br>默认值: false | 4.6.0 之前 |
| kNERtcSettingEnableDropBW | sdk.video.enable.drop.bandwidth | android<br>ios<br>mac<br>windows | 是否开启视频发送降带宽策略<br>默认值: false | 4.6.0 之前 |
| kNERtcSettingGdiBlackList | sdk.video.gdi_black_list | windows | 设置屏幕共享的 gdi 黑名单<br>默认值: 无 | 4.6.0 之前 |
| kNERtcSettingAllowWgcYellowFrame | sdk.video.allow.wgc_yellow_frame | windows | 是否允许屏幕共享中的 wgc yellow frame<br>默认值: true | 4.6.0 之前 |
| kNERtcSettingEnableWgc | sdk.video.enable.wgc | windows | 是否开启屏幕共享的 wgc<br>默认值: true | 4.6.0 之前 |
| kNERtcSettingForceWgcFilterMousecursor | sdk.video.force.wgc_capture_filter_mousecursor | windows | 是否强制屏幕共享的 wgc 过滤鼠标<br>默认值: false | 4.6.0 之前 |
| kNERtcSettingEnableMag | sdk.video.enable.mag | windows | 是否开启屏幕共享的 mag<br>默认值: true | 4.6.0 之前 |
| kNERtcSettingEnableDxgi | sdk.video.enable.dxgi | windows | 是否开启屏幕共享的 dxgi<br>默认值: true | 4.6.0 之前 |

### 3. audio

| Name | 定义 | 涉及的端 | 值类型及其含义 | 发布版本 |
| ---- | ---- | ---- | ---- | ---- |
| kNERtcSettingEnableAec | sdk.enable.aec | android: key_audio_aec_enable<br>ios: kNERtcKeyAudioProcessingAECEnable<br>mac/windows: audio_processing_aec_enable | 是否开启 AEC<br>默认值: true | 4.6.0 之前 |
| kNERtcSettingEnableLowLevelAec | sdk.enable.lowlevel.aec | mac/windows: audio_aec_low_level_enable | 是否开启音频 Lowlevel AEC <br>默认值: false | 4.6.0 之前 |
| kNERtcSettingEnableAgc | sdk.enable.agc | android: key_audio_agc_enable<br>ios: kNERtcKeyAudioProcessingAGCEnable<br>mac/windows: audio_processing_agc_enable | 是否开启 AGC<br>默认值: true | 4.6.0 之前 |
| kNERtcSettingEnableNs | sdk.enable.ns | android: key_audio_ns_enable<br>ios: kNERtcKeyAudioProcessingNSEnable<br>mac/windows: audio_processing_ns_enable | 是否开启 NS<br>默认值: true | 4.6.0 之前 |
| kNERtcSettingEnableAudioMix | sdk.enable.audio.mix | android: key_audio_external_audio_mix<br>ios: kNERtcKeyAudioProcessingExternalAudioMixEnable<br>mac/windows: audio_processing_external_audiomix_enable | 是否打开输入混音<br>默认值: true | 4.6.0 之前 |
| kNERtcSettingSetAudioCodecBitrate | sdk.audio.codec.bitrate | android/ios/mac/windows | 设置音频编码码率<br>默认值: 无 | 4.6.0 之前 |

## **Private API Keys**

### 1. channel
| Name | 定义 | 涉及的端 | 取值及含义 | 发布版本 |
| ---- | ---- | ---- | ---- | ---- |
| kNERtcSettingApiDisableGetChannelInfo | sdk.private.api.disable.get.channel.info | android/ios: sdk.disable.getChannelInfo | 关闭 get channel info<br>默认值: 无 | 4.6.21 |
| kNERtcSettingApiGetChannelInfoRequest | sdk.private.api.get.channel.info.request | android/ios: sdk.getChannelInfo.request<br> | 设置 get channel info 的请求<br>默认值: 无 | 4.6.21 |
| kNERtcSettingApiGetChannelInfoResponse | sdk.private.api.get.channel.info.response | android/ios: sdk.getChanneInfo.response<br> | 设置 get channel info 的回复<br>默认值: 无 | 4.6.21 |
| kNERtcSettingApiGetChannelInfoCustomData | sdk.private.api.get.channel.info.custom.data | android/ios: sdk.getChannelInfo.custom.data | 设置 get channel info 的定制数据<br>默认值: 无 | 4.6.21 |
| kLiteSDKSettingApiSetChannelScenarioType | sdk.private.api.set.channel.scenario.type | android/ios/mac/windows: sdk.new.channel.scenario.type | 房间模式 <br>默认值: 0 | 4.6.50 |

### 2. audio

| Name | 定义 | 涉及的端 | 取值及含义 | 发布版本 |
| ---- | ---- | ---- | ---- | ---- |
| kNERtcSettingApiResetAudioSession | sdk.private.api.reset.audio.session | ios: kNERtcKeyAudioSessionReset | 重置 iOS audio session 的设置<br>默认值: 无 | 4.6.0 之前 |