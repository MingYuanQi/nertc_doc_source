<!-- keywords: AudioSession, Audio Session, 没有声音 -->
本文介绍在使用 RTC SDK 时，Audio Session 配置的一些建议。

## Audio Session 简介
Audio Session 用于管理 App 对音频硬件设备（麦克风、扬声器）的资源使用。功能架构如下图所示。

![audiosession.png](https://yx-web-nosdn.netease.im/common/52651e9664e0a9cc37d7206d793cfcbc/audiosession.png)

Audio Session 的主要功能如下：

- 设置 App 的音频是否和其他 App 的音频同时存在，还是中断其他 App 的音频。
- 手机在静音模式下， App 的音频是否可以播放出声音。
- 电话或者其他 App 中断本 App 的音频时，该如何处理。
- 指定音频输入和输出的设备，例如使用听筒输出声音，还是扬声器输出声音。

## 使用建议

::: note important
建议在用户进入 RTC 房间后，不要执行任何修改 Audio Session 相关的操作。否则可能会出现无声等音频相关的问题。
:::

如果在使用 RTC 过程中有其他音频业务的需求，操作建议如下：
- 尽量不要设置 Audio Session，建议直接使用 RTC 的音频业务。
- 如果其他音频业务使用的是一个第三方 SDK，不确定内部是否会修改 Audio Session。可以先尝试，然后提供通话信息，联系技术支持确认。
- 如果确定需要业务层改变 Audio Session 才能实现音频业务，可以参考[操作步骤](#操作步骤)中的方法。

## 操作步骤

**在调用音频业务代码前，执行如下步骤：**
1. 调用 NERTC 的 [enableLocalAudio](https://doc.yunxin.163.com/nertc/api-refer/iOS/doxygen/Latest/zh/html/protocol_i_n_e_rtc_channel-p.html#a45c2df7b6d207e730c722b56540e588c) 接口，将参数值设置为 `NO`，关闭音频采集。
2. 获取当前 Audio Session 的配置，并保存下来。

    ```  
     AVAudioSession *audioSession = [AVAudioSession sharedInstance];
     //保存以下配置
     AVAudioSessionCategory category = audioSession.category;
     AVAudioSessionCategoryOptions categoryOptions = audioSession.categoryOptions;
     AVAudioSessionMode mode = audioSession.mode;
    ```
**在结束音频业务后，继续 RTC 通话前，执行如下步骤：**

3. 设置之前保存的 Audio Session 配置。

    ```
     NSError *error = nil;
     //设置category and option
    [[AVAudioSession sharedInstance] setCategory:category withOptions:categoryOptions error:&error];
     //设置mode
    [[AVAudioSession sharedInstance] setMode:mode error:&error];
     //设置option
    [[AVAudioSession sharedInstance] setCategory:category withOptions:categoryOptions error:&error];

    [[AVAudioSession sharedInstance] setActive:YES error:&error];
    ```

4. 调用 NERTC 的 [enableLocalAudio](https://doc.yunxin.163.com/nertc/api-refer/iOS/doxygen/Latest/zh/html/protocol_i_n_e_rtc_channel-p.html#a45c2df7b6d207e730c722b56540e588c) 接口，将参数值设置为 `YES
`，开启音频采集。

如果以上方法不能解决音频业务与云信 RTC 的兼容问题，请联系网易云信技术支持提供技术服务。

 