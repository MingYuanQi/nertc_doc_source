<!-- keywords: 实时音视频，音视频通话，webrtc，RTC，跑通示例项目 -->

您可以通过跑通 Sample Code，体验网易云信音视频通话功能，包括一对一音视频通话和多人音视频通话。

## <span id="开发环境">开发环境</span>

请确认您的开发环境满足以下要求:

- 安全环境：https 环境或者本地连接 localhost/127.0.0.1 环境。
- 浏览器：Chrome 72 及以上版本、Safari 12 及以上版本。更多浏览器兼容性相关请参考 [NERTC Web SDK 支持哪些浏览器](https://doc.yunxin.163.com/nertc/quick-start/TU5NjUzNjU?platform=web#Web%E7%AB%AF%E6%94%AF%E6%8C%81%E5%93%AA%E4%BA%9B%E6%B5%8F%E8%A7%88%E5%99%A8%E7%B1%BB%E5%9E%8B%E5%92%8C%E7%89%88%E6%9C%AC%EF%BC%9F)。

::: note notice
网易云信 Web SDK 仅支持 HTTPS 协议或 localhost（http://127.0.0.1）。请勿使用 HTTP 协议在 localhost 之外访问项目，否则 Web 浏览器控制台会报错 `NOT_SUPPORT 41001`。
:::

## <span id="前提条件">前提条件</span>

请确认您已完成以下操作：

- <a href="https://doc.yunxin.163.com/console/guide/TIzMDE4NTA?platform=console" target="_blank">创建应用并开获取 App Key</a>。
- <a href="https://doc.yunxin.163.com/nertc/quick-start/DA4NjQzNTU" target="_blank">开通音视频 2.0 服务</a>。


## <span id="操作步骤">操作步骤</span>

::: note notice
在运行示例项目之前，请在云信控制台中为指定应用<a href="https://doc.yunxin.163.com/nertc/quick-start/TQ0MTI2ODQ?platform=android#修改鉴权方式" target="_blank">开通调试模式</a>。调试模式建议只在集成开发阶段使用，请在应用正式上线前改回安全模式。
:::

1. 在<a href="https://github.com/netease-im/G2-API-Examples/tree/main/web/sampleCode-Vue/OneToOneVideoCall-Web-Vue" target="_blank">一对一通话示例代码</a>或<a href="https://github.com/netease-im/G2-API-Examples/tree/main/web/sampleCode-Vue/GroupVideoCall-Web-Vue" target="_blank">多人通话示例代码</a>下载页面下载需要体验的示例项目或 Demo 源码工程。

2. 在 `config/index.js` 文件中配置 App Key。
    ```
    export default {
        appkey: '', // 请输入自己的appkey
        appSecret: '' // 请输入自己的appSecret
    }
    ```

3. 运行项目。

    该工程使用 **vue（vue-cli 4.x）** 技术栈，请使用 **node** 开发环境 **version 16+**。 
    ::: note note
    若您仍需要使用 vue 5.x 版本，请联系技术支持。
    :::
    ```js
    // Project setup
    npm install

    // Compiles and hot-reloads for development
    npm run dev

    // Compiles and minifies for production
    npm run build

    // Lints and fixes files**
    npm run lint
    ```
4. 运行后访问 `https://localhost:8020/index.html#/?path=multiple` 地址体验 Web 端的音视频通话。

::: note note
为了帮您快速了解体验产品的基本音视频通话能力，网易云信还为您提供 NERTC SDK 的 [Web 端体验页面](https://app.yunxin.163.com/webdemo/g2-demo-wrapper/index.html)，包括一对一通话和多人通话，同时提供音视频质量采集、通话统计、旁路推流、伴音、屏幕共享等多种进阶功能。
:::