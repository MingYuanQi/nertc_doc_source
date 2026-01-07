音频 Dump 文件主要用于定位音频相关问题，本文介绍如何获取音频 Dump 文件。
## 操作步骤
1. 当房间内（joinChannel成功后，leaveChannel前）出现音频问题时，调用[`startAudioDumpWithType`](https://doc.yunxin.163.com/nertc/api-refer/iOS/doxygen/Latest/zh/html/protocol_i_n_e_rtc_engine_ex-p.html#a6719f39a56734ed36122f0e54de3ef13)接口，开始记录音频 Dump 信息。

    您可以调用 [`NERtcAudioDumpType`](https://doc.yunxin.163.com/nertc/api-refer/iOS/doxygen/Latest/zh/html/_n_e_rtc_engine_enum_8h.html#a4aba6d31fcf0862327fcf9a56937b374)，选择输出的 Dump 文件类型。
2. 收集完出现音频问题的 Dump 信息后，建议 2~3 分钟之后再调用[`stopAudioDump`](https://doc.yunxin.163.com/nertc/api-refer/iOS/doxygen/Latest/zh/html/protocol_i_n_e_rtc_engine_ex-p.html#a1ffe30ef9f592adb803c6277909cdf80) 接口，结束记录音频 Dump 信息。
3. 获取 Dump 文件，Dump 文件的目录如下表所示。
| 系统    | 存放目录                                                                | 日志目录的设置方法                               |
|---------|---------------------------------------------------------------------|---------------------------------------|
| Android | 存放在和 SDK 日志的同级目录中。                              | 调用 [NERtc.getInstance().init](https://doc.yunxin.163.com/docs/interface/NERTC_SDK/Latest/Android/html/classcom_1_1netease_1_1lava_1_1nertc_1_1sdk_1_1_n_e_rtc.html#a2198fb57cd127a8ae136e1f450236e57) 初始化时，通过 `NERtcOption` 对象的 `logDir` 参数指定日志路径。                     |
| iOS     | 存放在和 SDK 日志的同级目录中，命名为 `audioDump`。      | 调用 [setupEngineWithContext](https://doc.yunxin.163.com/nertc/api-refer/iOS/doxygen/Latest/zh/html/protocol_i_n_e_rtc_engine-p.html#aa0b59418e236407489b3a5cb7abdd9e1) 初始化时，通过 `NERtcEngineContext`.`NERtcLogSetting`.`logDir` 参数指定日志路径。 |
| Windows | 存放在和SDK日志的同级目录中，命名为 `netease_nrtc_audio.dmp`。 | 调用 [initialize](https://doc.yunxin.163.com/docs/interface/NERTC_SDK/Latest/PC/html/classnertc_1_1_i_rtc_engine.html#a1e816fd56f1cc6953a263f6798d0f1d4) 方法初始化时，通过 `NERtcEngineContext` 参数配置 `log_dir_path`， 指定日志目录的路径。        |
| macOS     | 存放在和 SDK 日志的同级目录中，命名为 `netease_nrtc_audio.dmp`。 | 调用 [initialize](https://doc.yunxin.163.com/docs/interface/NERTC_SDK/Latest/PC/html/classnertc_1_1_i_rtc_engine.html#a1e816fd56f1cc6953a263f6798d0f1d4) 方法初始化时，通过 `NERtcEngineContext` 参数配置`log_dir_path`，指定日志的路径。        |


4. 把 Dump 文件上传到自己的应用服务器目录上，打包后和故障现象一起反馈给网易云信技术支持工程师。