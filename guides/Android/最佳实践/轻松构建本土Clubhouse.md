<!-- keywords: Clubhouse，语聊房 -->
本文将从产品设计、技术实现以及在搭建中可能存在的技术难点几个维度，对 Clubhouse 进行全面的分析和解读。只需五步，即可轻松构建本土 Clubhouse。

## 架构设计

架构图如下：


![架构图.png](https://yx-web-nosdn.netease.im/common/2de85d03f4adfdf7574c49fa3428ad9f/架构图.png)



上图中，各组件说明如下：
- 客户端：封装实现客户端与应用服务 Clubhouse Server 的交互，封装实现与音视频的交互。
- 网关代理：应用服务的网关服务。
- Clubhouse Server：仿 Clubhouse 应用服务。
- RTC 音视频服务：提供稳定流畅、高品质、全平台的点对点和多人实时音视频通话服务，底层能力包括：

  - 网易云信 IM SDK
  - 网易云信 NERTC SDK


## 技术原理

除去用户标签、房间标签和话题推荐，Clubhouse 的功能大概分为以下几个板块：

- 房间列表
- 创建/加入房间
- 管理员邀请用户
- 举手发言
- 离开房间

其中，整体的房间控制需要在网易云信音视频通话 NERTC SDK 的基础之上，借助服务端来控制；加入房间后的音视频能力，则直接由 NERTC SDK 提供；另外服务端通知则由网易云信 IM SDK 提供的长链接服务来负责传递。

详细流程如下：
### 第一步：获取房间列表

调用服务端 `/clubRoom/list` 接口获取到房间列表。

![获取房间列表.png](https://yx-web-nosdn.netease.im/common/55c76aeaa376d8c0ac8e82e1192325e6/获取房间列表.png)


### 第二步：创建并加入房间

不论是创建房间还是加入房间，都会调用服务端提供的 `/clubRoom/join` 接口。在用户加入到房间时，应用服务器会判断 channelName 是否存在。
- 如果对应的 channelName 不存在，会创建一个房间并加入同时返回相应的房间信息。
- 如果对应的 channelName 存在，则用户直接加入该房间。

当获取到服务端返回的房间信息时，再调用 NERTC SDK 的加入房间 API [`joinChannelWithToken`](https://dev.yunxin.163.com/docs/interface/NERTC_SDK/Latest/iOS/html/protocol_i_n_e_rtc_engine-p.html#adf60d9392e4b50ff73872138965f022f)，真正加入音频房间。

当加入房间成功后，NERTC SDK 会抄送消息至应用服务器，更新用户在房间中的状态。


![创建并加入房间.png](https://yx-web-nosdn.netease.im/common/097ce717cb50a53c6196da6f29b99558/创建并加入房间.png)

### 第三步：管理员邀请用户加入房间

当管理员点击邀请用户加入房间时，会先获取到好友列表，然后服务端生成一个短链返回到客户端。当被邀请者点击短链后，会自动加入房间。



![管理员邀请用户加入房间.png](https://yx-web-nosdn.netease.im/common/ef0f349082f80e403c83a56126a4ee3c/管理员邀请用户加入房间.png)


### 第四步：举手发言

1. 客户端会先调用 `/clubRoom/handsup` 接口，告诉服务端我想发言。
2. 服务端通过云信 IM 提供的透传协议以及长链接将消息发送给房间管理员。
3. 管理员点击同意时，会调用管理员会控接口 `/clubRoom/control/host` 更新成员音频状态为**发言状态**，同时应用服务器通过 IM 透传协议通知举手者音频已打开，此时举手者调用 NERTC 的 [`enableLocalAudio`](https://dev.yunxin.163.com/docs/interface/NERTC_SDK/Latest/iOS/html/protocol_i_n_e_rtc_channel-p.html#a45c2df7b6d207e730c722b56540e588c) 接口来开启麦克风。


![举手发言.png](https://yx-web-nosdn.netease.im/common/bc542cbb8dde667493e8c29469c89386/举手发言.png)

![管理员同意成员举手发言.png](https://yx-web-nosdn.netease.im/common/e370679c751cac694dd2dd095c392b3f/管理员同意成员举手发言.png)


### 第五步：离开房间

当用户点击离开房间按钮后，直接调用 NERTC SDK 的 [`leaveChannel`](https://dev.yunxin.163.com/docs/interface/NERTC_SDK/Latest/iOS/html/protocol_i_n_e_rtc_channel-p.html#a2dc58957c2aaec792558ec411a2eb238) 方法离开房间，此时，NERTC 会抄送用户离开消息至应用服务器，服务器标记该用户离开。


![离开房间.png](https://yx-web-nosdn.netease.im/common/a7accd863ca0f8e576b1a10866bb631d/离开房间.png)

## 实现方法
1. 导入类

在项目中导入 NERtcSDK 类：
```
import com.netease.lava.nertc.sdk.NERtcCallbackEx;
import com.netease.lava.nertc.sdk.NERtcConstants;
import com.netease.lava.nertc.sdk.NERtcEx;
import com.netease.lava.nertc.sdk.NERtcParameters;
import com.netease.lava.nertc.sdk.video.NERtcRemoteVideoStreamType;
import com.netease.lava.nertc.sdk.video.NERtcVideoView;
```

2. 初始化

打开 App 后，先执行如下方法完成初始化。
```
// 示例
private void initializeSDK() {
      try {
          NERtcEx.getInstance().init(getApplicationContext(),Config.APP_KEY,callback,null);
      } catch (Exception e) {
          showToast("SDK初始化失败");
          finish();
          ...
          return;
      }
      ...
} 
```

3. 加入房间

加入房间前，请确保已完成初始化相关事项。通过 [`joinChannel`](https://doc.yunxin.163.com/nertc/api-refer/android/doxygen/Latest/zh/html/classcom_1_1netease_1_1lava_1_1nertc_1_1sdk_1_1channel_1_1_n_e_rtc_channel.html#a312298f0950182a9844562c8bb359e40) 方法加入房间。
```
public abstract int joinChannel(java.lang.String token,
                                java.lang.String channelName,
                                            long uid);

```
4. 退出通话房间

通过 [`leaveChannel`](https://doc.yunxin.163.com/nertc/api-refer/android/doxygen/Latest/zh/html/classcom_1_1netease_1_1lava_1_1nertc_1_1sdk_1_1channel_1_1_n_e_rtc_channel.html#aa01418f1cdb24d74e44f9c2cb3cad5fe) 接口退出通话房间。
```
// 示例
  // 退出通话房间
  NERtcEx.getInstance().leaveChannel();

```
NERtcCallback 提供 [`onLeaveChannel()`](https://doc.yunxin.163.com/nertc/api-refer/android/doxygen/Latest/zh/html/interfacecom_1_1netease_1_1lava_1_1nertc_1_1sdk_1_1_n_e_rtc_callback.html#a8c4cbc5219d80e3dedf66d0570d470a4) 接口来监听当前用户退出房间的结果。

## 相关参考
- [多人语音聊天室示例项目源码](https://github.com/netease-im/NEChatroom)
- [多人语音聊天室场景方案文档](https://doc.yunxin.163.com/docs/DM0NTczODE/TMzNTAwODU?platformId=20002)
## 技术难点分析
### 音频技术难点与解决方案
**问题描述**

- 弱网情况下的丢包问题
- 设备适配问题
- 音质问题

**解决方案**

1. 云信音视频通话 2.0 使用自研的网络引擎弱网算法，保证在 80% 丢包的传输场景下，音频也能进行正常通话，弱网优势更明显。

2. 云信针对超过数千款设备进行音质适配，保证回声抑制的效果在绝大多数机型上都有最优的表现。

3. 自研的音频 AI 降噪算法，可以针对嘈杂人声、键盘声等非稳态噪声进行定向降噪，提升对于环境稳态噪声的抑制能力，保留更纯粹人声。

### 内容管控技术难题与解决方案

**问题描述**

- 对于 Clubhouse 这一类声音社交的语音聊天室场景，场景中可能出现如暴恐、涉政、色情、广告等不可控违规内容。随着有关部门的监管力度不断增强，平台对于内容进行管控的工作成为了必要。

- 实时音频场景下的内容审核，由于其场景实时进行的特殊性，对反垃圾服务也提出了较为严苛的要求。例如，审核结果必须足够实时，嘈杂场景下的音频采集不能严重影响检出率，高并发场景下需要做到快速响应不拥塞等等。


**解决方案**

云信针对该场景打磨出了一套完备的实时音频反垃圾服务，为客户的业务合规性保驾护航。

该服务通过业内领先的语音识别技术，结合反垃圾文本过滤规则体系，精准、高效分析识别违规音频。此外，依托网易云计算资源，动态扩容，弹性伸缩，满足客户的涉黄、涉政、广告等其他多维度场景的高并发、高精准的反垃圾检测。

具体实现方式请参见[实现音视频安全检测](https://doc.yunxin.163.com/docs/jcyOTA0ODM/DY2Mjk4MTY?platformId=50192)。


![安全通.png](https://yx-web-nosdn.netease.im/common/24e7a6a3e3136c497f6e0d4d5a1eaa16/安全通.png)


