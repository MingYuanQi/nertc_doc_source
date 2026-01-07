网易云信音视频通话 2.0（NERTC）实时字幕功能允许用户在音视频通话中，实时显示语音转文本的字幕，而字幕翻译支持将实时字幕同时翻译为目标语言。本文详细介绍如何开启实时字幕、开启字幕翻译、关闭实时字幕功能，并提供示例代码和接口参考。

<style>
table th:first-of-type {
    width: 30%;
}
</style>

## 功能特性

字幕功能适用于将语音实时转换为文本字幕，字幕翻译功能适用于多语言会议、跨国交流、无障碍沟通、在线教育课程、会议记录与归档等场景。

- **实时多语种字幕**：将语音即时转换为精准文本字幕，支持多种主流语言。
- **智能语言翻译**：自动将字幕翻译成目标语言，实现跨语言无缝沟通。
- **状态回调**：提供字幕开启、关闭、失败等状态回调。
- **字幕展示**：支持自定义字幕展示样式和位置。
- **字幕下载**：支持通过服务端 [云端录制事件抄送](https://doc.yunxin.163.com/nertc/server-apis/Tg5OTI0ODA?platform=server#3) 下载 `.txt` 字幕文件。

## 准备工作

根据本文操作前，请确保您已经完成了以下设置：

1. **开通功能权限**：如果您需要使用实时字幕和字幕翻译功能，请联系您的网易云信客户经理或 [提交工单](https://app.yunxin.163.com/global/service/ticket/create) 联系网易云信技术支持工程师开通。
2. **集成 NERTC SDK**：项目已集成最新版本的 NERTC SDK，推荐 5.6.50 及以上版本。详情请参考 [集成 SDK（音视频）](https://doc.yunxin.163.com/nertc/guide/DI3NDEyNDI?platform=macOS)。
3. **初始化 NERTC SDK**：在使用实时字幕和字幕翻译功能前，初始化 NERTC SDK 并将用户加入音视频房间。详情请参考 [实现音视频通话](https://doc.yunxin.163.com/nertc/guide/TU1OTc0NzM?platform=macOS#%E6%AD%A5%E9%AA%A4%E4%B8%89-%E5%88%9D%E5%A7%8B%E5%8C%96)。

    ```C++
    //初始化 SDK
    NERtcEngineContext context;
    context.app_key = "xxxxxxxxxxxx";
    context.event_handler = this;
    IRtcEngineEx *rtc_engine = (IRtcEngineEx *)createNERtcEngine();
    rtc_engine->initialize(context);

    //加入 RTC 房间
    rtc_engine_->joinChannel(token, channel_name, uid);

    void onJoinChannel(channel_id_t cid, uid_t uid, NERtcErrorCode result, uint64_t elapsed) {
    //加入房间的结果返回
    }

    void onUserJoined(uid_t uid, const char* user_name, NERtcUserJoinExtraInfo join_extra_info) {
    //远端加入房间
    }

    ```

## 场景一：开启实时字幕

您可以在完成准备工作后，调用 [`startASRCaption`](https://doc.yunxin.163.com/nertc/references/macOS/doxygen/Latest/zh/html/classnertc_1_1_i_rtc_engine_ex.html#a1689a0bdcf317353d05e806bcb4140d7) 接口开启实时字幕功能。功能调用流程如下图所示，涉及的接口请参考下文 [相关接口](#apiList) 章节：

```mermaid
sequenceDiagram
    autonumber
    participant App as 应用程序
    participant SDK as NERTC SDK
    participant Server as NERTC 服务器

    par 准备工作
    App->>SDK: 1. 初始化 NERTC SDK，添加 RTC 相关的事件监听
    App->>SDK: 2. 加入房间
    end
    par 启用功能
    App->>SDK: 3. 开启实时字幕 <br>(startASRCaption)<br>并设置源语言和目标语言
    SDK->>Server: 请求开启字幕服务
    Server-->>SDK: 返回字幕服务状态
    SDK-->>App: 4. 监听回调，返回字幕开启结果<br> (onAsrCaptionStateChanged)
    Server->>SDK: 发送字幕内容
    SDK-->>App: 监听回调，返回字幕状态和内容<br> (onAsrCaptionResult)
    end
```

相关示例代码如下所示：

```C++
//开启实时字幕
NERtcASRCaptionConfig asr_config;
asr_config.src_language = "AUTO"; // 源语言，默认为 `AUTO`，表示自动识别中英文。
// asr_config.dst_language = "EN"; // 如需开启字幕翻译，您需要设置目标语言。
rtc_engine->startASRCaption(asr_config);

// 监听字幕状态
void onAsrCaptionStateChanged(NERtcAsrCaptionState state, int code, const char* message) {
    if (state == kNERtcAsrCaptionStartFailed) {
     //开启字幕失败
    } else if (state == kNERtcAsrCaptionStateStopFailed) {
     //关闭字幕失败
    } else if (state == kNERtcAsrCaptionStateStarted) {
     //开启字幕成功
    } else if (state == kNERtcAsrCaptionStateStopped) {
     //关闭字幕成功
    }
}

// 监听字幕内容
void onAsrCaptionResult(const NERtcAsrCaptionResult *results, unsigned int result_count) {
    for (int i = 0; i < result_count; ++i) {
        auto content = results[i].content;
        //此处可以根据解析的 content 进行页面展示
    }
}
```

**参数说明**

| 参数 | 说明 |
| --- | --- |
| [`NERtcASRCaptionConfig`](https://doc.yunxin.163.com/nertc/references/macOS/doxygen/Latest/zh/html/structnertc_1_1_n_e_rtc_a_s_r_caption_config.html) | 字幕语言配置对象。 |
    `srcLanguage` | 字幕的源语言，默认为 `AUTO`，支持的语言列表请参考下文 [语言列表](#language)。 |
    `dstLanguage` | 字幕翻译的目标语言，支持的语言列表请参考下文 [语言列表](#language)。 |
| [`asrState`](https://doc.yunxin.163.com/nertc/references/macOS/doxygen/Latest/zh/html/namespacenertc.html#ac042e82e46edd17c73cb378979cb7435) | 请求开启字幕的状态： |\
| | - **0**：请求开启字幕失败，建议 App 重新调用接口开启字幕。 |\
| | - **1**：请求关闭字幕失败，建议 App 重新调用接口关闭字幕。 |\
| | - **2**：请求开启字幕成功，App 可以接收字幕内容的回调。 |\
| | - **3**：请求关闭字幕成功，App 停止接收字幕内容的回调。 |
| `code` | 状态码，详情请参考 [开启实时音频互动](https://doc.yunxin.163.com/nertc/server-apis/jQzOTE2NTc?platform=server#%E7%8A%B6%E6%80%81%E7%A0%81) 状态码章节。 |

## 场景二：开启字幕翻译

### 开启方式

开启实时字幕功能后，可以开启字幕翻译功能。您只需要调用 `startASRCaption` 接口开启实时字幕翻译功能，并设置源语言和目标语言。

```C++
//开启字幕翻译
NERtcASRCaptionConfig asr_config;
asr_config.src_language = "AUTO"; // 源语言，默认为 `AUTO`，表示自动识别中英文。
asr_config.dst_language = "EN"; // 如需开启字幕翻译，您需要设置目标语言。
rtc_engine->startASRCaption(asr_config);
```

### 语言设置

翻译字幕时，源语言（`src_language`）和目标语言（`dst_language`）需要根据用户的说话语言和期望看到的翻译语言分别设置这两个参数，而非根据通话对方的语言来设置。

- **src_language (源语言)**：指的是 **本端用户说话使用的语言**，即需要被识别和转换为文字的语音所使用的语言。
- **dst_language (目标语言)**：指的是 **本端用户希望看到的翻译后的语言**，即字幕翻译后展示的语言。

```mermaid
graph TD
    %% 定义节点
    A["用户A说<br>"你好""] -->|语音| B["AI 字幕与<br>翻译服务"]
    B -->|用户A的字幕| C["你好"]
    B -->|翻译给用户A| C1["再见"]

    A1["用户B说<br>"Bye""] -->|语音| B
    B -->|翻译给用户B| D["Hello"]
    B -->|用户B的字幕| D1["Bye"]

    subgraph config4["设置效果"]
    A1
    D
    D1
    end

    subgraph config3["设置效果"]
    A
    C
    C1
    end

    %% 配置子图
    subgraph config1["用户B配置"]
    G["srcLanguage=en<br>说英文"]
    H["dstLanguage=en<br>翻译外语成英文"]
    end

    subgraph config2["用户A配置"]
    E["srcLanguage=zh<br>说中文"]
    F["dstLanguage=zh<br>翻译外语成中文"]
    end

    %% 样式设置
    style A fill:#f0f9ff,stroke:#69c0ff,stroke-width:2px
    style A1 fill:#f9f0ff,stroke:#b37feb,stroke-width:2px
    style B fill:#f6ffed,stroke:#52c41a,stroke-width:2px
    style C fill:#e6f7ff,stroke:#1890ff,stroke-width:1px
    style D fill:#f9f0ff,stroke:#722ed1,stroke-width:1px
    style C1 fill:#e6f7ff,stroke:#1890ff,stroke-width:1px
    style D1 fill:#f9f0ff,stroke:#722ed1,stroke-width:1px
    
    style E fill:#e6f7ff,stroke:#1890ff,stroke-width:2px
    style F fill:#e6f7ff,stroke:#1890ff,stroke-width:2px
    style G fill:#f9f0ff,stroke:#722ed1,stroke-width:2px
    style H fill:#f9f0ff,stroke:#722ed1,stroke-width:2px
    
    %% 子图样式
    classDef subgraphStyle fill:#fafafa,stroke:#d9d9d9,stroke-width:1px
    class config1 subgraphStyle
    class config2 subgraphStyle
    class config3 subgraphStyle
    class config4 subgraphStyle
```

### 示例一：不同语言日常交流

- 中国用户（说中文，并需要把对方说的英文翻译成中文）设置：

    ```C++
    NERtcASRCaptionConfig asr_config;
    asr_config.src_language = "zh"; // 源语言设为中文，因为我说中文
    asr_config.dst_language = "zh"; // 目标语言设为中文，所以我看到对方说话的英文会被翻译成中文
    rtc_engine->startASRCaption(asr_config);
    ```

- 美国用户（说英文，并需要把对方说的中文翻译成英文）设置：

    ```C++
    NERtcASRCaptionConfig asr_config;
    asr_config.src_language = "en"; // 源语言设为英文，因为我说英文
    asr_config.dst_language = "en"; // 目标语言设为英文，所以我看到对方说话的中文会被翻译成英文
    rtc_engine->startASRCaption(asr_config);
    ```

### 示例二：多国语言会议

- 中文用户设置：

    ```C++
    asr_config.src_language = "zh"; // 我说中文
    asr_config.dst_language = "zh"; // 需要把对方说的语言翻译成中文
    ```

- 英文用户设置：

    ```C++
    asr_config.src_language = "en"; // 我说英文
    asr_config.dst_language = "en"; // 需要把对方说的语言翻译成英文
    ```

- 日文用户设置：

    ```C++
    asr_config.src_language = "ja"; // 我说日文
    asr_config.dst_language = "ja"; // 需要把对方说的语言翻译成日文
    ```

## 场景三：关闭实时字幕

调用 `stopASRCaption` 接口关闭实时字幕功能。

```C++
//关闭实时字幕
rtc_engine->stopASRCaption();
```

<a id="apiList"></a>

## 相关接口

本文涉及的接口和事件如下所示：

| 接口/事件 | 说明 |
| --- | --- |
| [`init`](https://doc.yunxin.163.com/nertc/api-refer/macOS/doxygen/Latest/zh/html/classnertc_1_1_i_rtc_engine.html#a1e816fd56f1cc6953a263f6798d0f1d4) | 初始化 NERTC SDK。 |
| [`joinChannel`](https://doc.yunxin.163.com/nertc/api-refer/macOS/doxygen/Latest/zh/html/classnertc_1_1_i_rtc_engine.html#acc3f404cee7cd1b56b5019776dbb660a) | 加入音视频房间。 |
| [`startASRCaption`](https://doc.yunxin.163.com/nertc/api-refer/macOS/doxygen/Latest/zh/html/classnertc_1_1_i_rtc_engine_ex.html#a1689a0bdcf317353d05e806bcb4140d7) | 开启实时字幕功能。 |
| [`stopASRCaption`](https://doc.yunxin.163.com/nertc/api-refer/macOS/doxygen/Latest/zh/html/classnertc_1_1_i_rtc_engine_ex.html#a233033d4fff11b7c8160e3507235c426) | 关闭实时字幕功能。 |
| [`onJoinChannel`](https://doc.yunxin.163.com/nertc/api-refer/macOS/doxygen/Latest/zh/html/classnertc_1_1_i_rtc_engine_event_handler.html#a3f55353db4a1369d70ec3859e91c7337) | 本端加入房间的回调。 |
| [`onUserJoined`](https://doc.yunxin.163.com/nertc/api-refer/macOS/doxygen/Latest/zh/html/classnertc_1_1_i_rtc_engine_event_handler.html#a84a5b3eb47d7f0465082db4bd4bd4989) | 远端用户加入房间的回调。 |
| [`onAsrCaptionStateChanged`](https://doc.yunxin.163.com/nertc/references/macOS/doxygen/Latest/zh/html/classnertc_1_1_i_rtc_engine_event_handler_ex.html#a2b6b9377a15928ec24249278199e8d42) | 字幕状态变化的回调。 |
| [`onAsrCaptionResult`](https://doc.yunxin.163.com/nertc/references/macOS/doxygen/Latest/zh/html/classnertc_1_1_i_rtc_engine_event_handler_ex.html#adf82bb032c38c1ce42ea7d8a663113ee) | 收到字幕内容的回调。 |

<a id="language"></a>

## 语言列表

`startASRCaption` 接口中源语言 [`srcLanguage`](https://doc.yunxin.163.com/nertc/references/macOS/doxygen/Latest/zh/html/structnertc_1_1_n_e_rtc_a_s_r_caption_config.html#a56ca367396f19ec28f760311dbf17b5f) 和目标语言 [`dstLanguage`](https://doc.yunxin.163.com/nertc/references/macOS/doxygen/Latest/zh/html/structnertc_1_1_n_e_rtc_a_s_r_caption_config.html#a85272f758555e4f8211c43efe42d64ec) 支持的语言代码列表如下所示：

| 语言类型 | 语言代码 | 名称 | 源语言 | 目标语言 |
|---|:---:|---|---|---|
| 常见语言 | AUTO | 中英文自动识别 | ✔️ | - |
| ^^ | ar | 阿拉伯语 | ✔️ | ✔️ |
| ^^ | de | 德语 | ✔️ | ✔️ |
| ^^ | en | 英语 | ✔️ | ✔️ |
| ^^ | es | 西班牙语 | ✔️ | ✔️ |
| ^^ | fr | 法语 | ✔️ | ✔️ |
| ^^ | hi | 印地语 | ✔️ | ✔️ |
| ^^ | id | 印度尼西亚语 | ✔️ | ✔️ |
| ^^ | it | 意大利语 | ✔️ | ✔️ |
| ^^ | ja | 日语 | ✔️ | ✔️ |
| ^^ | ko | 韩语 | ✔️ | ✔️ |
| ^^ | nl | 荷兰语 | ✔️ | ✔️ |
| ^^ | pt | 葡萄牙语 | ✔️ | ✔️ |
| ^^ | ru | 俄语 | ✔️ | ✔️ |
| ^^ | th | 泰语 | ✔️ | ✔️ |
| ^^ | vi | 越南语 | ✔️ | ✔️ |
| ^^ | zh | 简体中文 | ✔️ | ✔️ |
| 汉语方言 | db | 东北话 | ✔️ | - |
| ^^ | nan | 闽南语 | ✔️ | - |
| ^^ | sc | 四川话 | ✔️ | - |
| ^^ | yue | 粤语 | ✔️ | - |
| ^^ | zj | 浙江话 | ✔️ | - |
| 非常见语言 | bn | 孟加拉语 | ✔️ | ✔️ |
| ^^ | fa | 波斯语 | ✔️ | ✔️ |
| ^^ | he | 希伯来语 | ✔️ | ✔️ |
| ^^ | km | 高棉语 | ✔️ | ✔️ |
| ^^ | lo | 老挝语 | ✔️ | ✔️ |
| ^^ | ms | 马来语 | ✔️ | ✔️ |
| ^^ | tl | 菲律宾语 | ✔️ | ✔️ |
| ^^ | tr | 土耳其语 | ✔️ | ✔️ |

## 常见问题

### 服务端如何获取翻译后的字幕文件？

您可以通过 [云端录制事件抄送（`eventType` = `3`）](https://doc.yunxin.163.com/nertc/server-apis/Tg5OTI0ODA?platform=server#3) 获取翻译后的 `.txt` 字幕文件。

### 开启实时字幕失败怎么办？

- 检查是否已初始化 NERTC SDK 并成功加入房间。
- 确认音频配置是否正确，应用权限（麦克风）是否已正确授予。
- 检查网络连接是否正常。
- 验证您的网易云信开发者账号的应用密钥（`APP_KEY`）是否有效且开启了字幕服务权限。

### 翻译不准确怎么办？

- 请确保您选择了正确的源语言和目标语言。
- 清晰、规范地发音有助于提高识别率，同时使用外置麦克风可提高录音质量。
- 专业术语可能需要手动校正。

### 字幕显示延迟较高怎么办？

- 确保网络环境良好，避免高延迟或丢包。
- 调整音频配置，使用较低的采样率和码率。

### 如何自定义字幕样式？

- 在 `onAsrCaptionResult` 回调中获取字幕内容后，使用自定义 UI 组件展示字幕。