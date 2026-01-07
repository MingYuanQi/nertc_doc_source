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
2. **集成 NERTC SDK**：项目已集成最新版本的 NERTC SDK，推荐 5.6.50 及以上版本。详情请参考 [集成 SDK（音视频）](https://doc.yunxin.163.com/nertc/guide/DM3MDA4NzQ?platform=web)。
3. **初始化 NERTC SDK**：在使用实时字幕和字幕翻译功能前，初始化 NERTC SDK 并将用户加入音视频房间。详情请参考 [实现音视频通话](https://doc.yunxin.163.com/nertc/guide/TA1NTk0NzY?platform=web#%E7%AC%AC%E4%B8%89%E6%AD%A5%E5%88%9D%E5%A7%8B%E5%8C%96)。

    ```JavaScript
    const client = NERTC.createClient({
    appkey,
    });

    await client.join({
        channelName,
        uid,
        token,
    })
    const localStream = NERTC.createStream({
        audio: true,
        video: true,
        }) ;
    await localStream.init()

    await client.publish(localStream);
    ```

## 场景一：开启实时字幕

您可以在完成准备工作后，调用 [`startAsrCaptions`](https://doc.yunxin.163.com/docs/interface/nertc/web/typedoc/Latest/zh/html/interfaces/client.client-1.html#startasrcaptions) 接口开启实时字幕功能。功能调用流程如下图所示，涉及的接口请参考下文 [相关接口](#apiList) 章节：

```mermaid
sequenceDiagram
    autonumber
    participant App as 应用程序
    participant SDK as NERTC SDK
    participant Server as NERTC 服务器

    par 准备工作
    App->>SDK: 初始化 NERTC SDK，发布音频流
    App->>SDK: 加入房间
    end
    par 启用功能
    App->>SDK: 开启实时字幕 <br>(client.startAsrCaptions)<br>并设置源语言和目标语言
    SDK->>Server: 请求开启字幕服务
    Server->>SDK: 发送字幕内容
    SDK-->>App: 通过 client.on('asr-captions') 回调<br>返回字幕状态和内容
    end
```

相关示例代码如下所示：

```JavaScript
//开启实时字幕
await client.startAsrCaptions();
//字幕回调
client.on('asr-captions', (data) => {
    data.forEach((item) => {
      const caption = {} as any;
      const { timestamp, srcUid, text, isFinal } = item;
      //处理字幕
      //timestamp 时间戳
      //srcUid 该条字幕对应的 uid
      //text 字幕文本
      //isFinal 表示这一句话是否讲完
    })
 });
```

## 场景二：开启字幕翻译

### 开启方式

开启实时字幕功能后，可以开启字幕翻译功能。您只需要调用 `startAsrCaptions` 接口开启实时字幕翻译功能，并设置源语言和目标语言。

```JavaScript
// 开启字幕+翻译
// 第一个参数表示源语言，设置为 `AUTO`，表示自动识别中英文。
// 第二个参数表示目标语言
await client.startAsrCaptions('AUTO', "EN");
//字幕回调
client.on('asr-captions', (data) => {
    data.forEach((item) => {
      const caption = {} as any;
      const { timestamp, srcUid, text, isFinal, hasTrans, transLanguage, transText } = item;
      //处理字幕+翻译
      //timestamp 时间戳
      //srcUid 该条字幕对应的 uid
      //text 字幕文本
      //isFinal 表示这一句话是否讲完
      //hasTrans 是否存在翻译
      //transLanguage 目标语言
      //transText 翻译结果
    })
});
```

### 语言设置

翻译字幕时，源语言（第一个参数）和目标语言（第二个参数）需要根据用户的说话语言和期望看到的翻译语言分别设置这两个参数，而非根据通话对方的语言来设置。

- **源语言**：指的是 **本端用户说话使用的语言**，即需要被识别和转换为文字的语音所使用的语言。
- **目标语言**：指的是 **本端用户希望看到的翻译后的语言**，即字幕翻译后展示的语言。

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

    ```JavaScript
    // 开启字幕+翻译
    await client.startAsrCaptions('zh', 'zh'); // 源语言设为中文，目标语言设为中文
    ```

- 美国用户（说英文，并需要把对方说的中文翻译成英文）设置：

    ```JavaScript
    // 开启字幕+翻译
    await client.startAsrCaptions('en', 'en'); // 源语言设为英文，目标语言设为英文
    ```

### 示例二：多国语言会议

- 中文用户设置：

    ```JavaScript
    await client.startAsrCaptions('zh', 'zh'); // 我说中文，需要把对方说的语言翻译成中文
    ```

- 英文用户设置：

    ```JavaScript
    await client.startAsrCaptions('en', 'en'); // 我说英文，需要把对方说的语言翻译成英文
    ```

- 日文用户设置：

    ```JavaScript
    await client.startAsrCaptions('ja', 'ja'); // 我说日文，需要把对方说的语言翻译成日文
    ```

## 场景三：关闭实时字幕

调用 [`stopAsrCaptions`](https://doc.yunxin.163.com/docs/interface/nertc/web/typedoc/Latest/zh/html/interfaces/client.client-1.html#stopasrcaptions) 接口关闭实时字幕功能。

```JavaScript
// 关闭实时字幕
await client.stopAsrCaptions();
```

<a id="apiList"></a>

## 相关接口

本文涉及的接口和事件如下所示：

| 接口/事件 | 说明 |
| --- | --- |
| [client.join](https://doc.yunxin.163.com/docs/interface/nertc/web/typedoc/Latest/zh/html/interfaces/client.client-1.html#join) | 加入音视频通话房间 |
| [stream.setAudioProfile](https://doc.yunxin.163.com/docs/interface/nertc/web/typedoc/Latest/zh/html/interfaces/stream.stream-1.html#setaudioprofile) | 设置音频属性 |
| [stream.init](https://doc.yunxin.163.com/docs/interface/nertc/web/typedoc/Latest/zh/html/interfaces/stream.stream-1.html#init) | 初始化本地媒体流 |
| [client.publish](https://doc.yunxin.163.com/docs/interface/nertc/web/typedoc/Latest/zh/html/interfaces/client.client-1.html#publish) | 发布本地媒体流 |
| [client.startAsrCaptions](https://doc.yunxin.163.com/docs/interface/nertc/web/typedoc/Latest/zh/html/interfaces/client.client-1.html#startasrcaptions) | 开启实时字幕功能 |
| [client.on](https://doc.yunxin.163.com/docs/interface/nertc/web/typedoc/Latest/zh/html/interfaces/client.client-1.html#on) | 注册监听事件 |
| [client.stopAsrCaptions](https://doc.yunxin.163.com/docs/interface/nertc/web/typedoc/Latest/zh/html/interfaces/client.client-1.html#stopasrcaptions) | 关闭实时字幕功能 |


<a id="language"></a>

## 语言列表

`startAsrCaptions` 接口中源语言 `srcLanguage` 和目标语言 `dstLanguage` 支持的语言代码列表如下所示：

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

- 在 `client.on` 回调中获取字幕内容后，使用自定义 UI 组件展示字幕。