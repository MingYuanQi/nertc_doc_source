
<!--和互动直播对应文档基本相同，替换产品名称后即可重用-->

本文为您介绍 Web 端集成 SDK 的操作步骤，帮助您快速集成 SDK，开始使用音视频通话的基本功能。

## <span id="前提条件">前提条件</span>

在开始运行工程之前，请您准备以下开发环境：

- 安全环境：https 环境或者本地连接 localhost/127.0.0.1 环境。
- 浏览器：Chrome 72 及以上版本、Safari 12 及以上版本。更多浏览器兼容性相关请参考 [NERTC Web SDK 支持哪些浏览器](https://doc.yunxin.163.com/nertc/quick-start/TU5NjUzNjU?platform=web#Web%E7%AB%AF%E6%94%AF%E6%8C%81%E5%93%AA%E4%BA%9B%E6%B5%8F%E8%A7%88%E5%99%A8%E7%B1%BB%E5%9E%8B%E5%92%8C%E7%89%88%E6%9C%AC%EF%BC%9F)。

::: note notice
- 网易云信 Web SDK 仅支持 HTTPS 协议或 localhost（http://127.0.0.1）。请勿使用 HTTP 协议在 localhost 之外访问项目，否则 Web 浏览器控制台会报错 `NOT_SUPPORT 41001`。

- 使用PC端Chrome浏览器模拟移动端调试时，浏览器内核版本会根据不同的机型改变，注意要使用Chrome72及以上的版本。具体的内核版本请查看User-Agent
:::

## 集成 SDK

### npm 集成

1. 安装 npm。

    操作步骤请参考 <a href="https://www.npmjs.cn/getting-started/installing-node/" target="_blank">npm 官方文档</a>。

2. 通过 vue 搭建一个简单界面的项目，并在项目中使用 npm 安装 SDK 包。
    
    ```
    npm install nertc-web-sdk --save
    ```
    ::: note notice
    - 在 vue 的项目配置中需设置外部引用 NERTC Web SDK，请勿使用 babel 打包 SDK。
    - 如果您在 macOS 或 Linux 系统中执行 npm 命令失败，报错 `permission denied`，请在 npm 命令前加上 `sudo` 并重新执行即可。</font>
    :::

3. 在项目脚本里引入模块。

    ::: note note
    **若您对包体积大小有要求**，请集成 V4.6.20 及之后版本的 NERTC SDK，实现以插件化的方式按需集成动态库。
    :::

    - 若您集成的是 **v4.6.20 及之后版本**的 NERTC SDK，请添加如下代码。
    ```
    import NERTC from "nertc-web-sdk/NERTC"
    ```
    
    - 若您集成的是 **v4.6.20 之前版本**的 NERTC SDK，请添加如下代码。
    ```
    import NERTC from "nertc-web-sdk"
    ```

4. 在本地 Web 服务器上运行和测试项目。</font>

    ::: note notice
    本地服务器（localhost 或 127.0.0.1）运行 Web 应用仅作为测试，生产环境必须使用 https 协议。
    :::

### 手动集成
    
1. 在<a href="https://doc.yunxin.163.com/nertc/sdk-download" target="_blank">网易云信 SDK 下载中心</a>获取当前最新版本的 NERTC Web SDK，或联系网易云信技术支持获取对应版本的 SDK。

2. 通过 vue 搭建一个简单界面的项目。

3. 导入 SDK。

    ::: note note
    **若您对包体积大小有要求**，请集成 V4.6.20 及之后版本的 NERTC SDK，实现以插件化的方式按需集成动态库。
    :::

    - 若您集成的是 **v4.6.20 及之后版本**的 NERTC SDK，解压 SDK 压缩包后，请参考下表按需将文件拷贝到项目文件所在目录下。

    <table> 
    <tbody>
    <tr> 
        <th width="10%"><b>功能/插件</b></th> 
        <th width="20%"><b>集成的动态库</b></th>
        <th width="10%"><b>是否可选</b></th> 
    </tr>
    <tr> 
        <td>音视频</td> 
        <td>NIM_Web_NERTC_vx.x.x.js</td> 
        <td>必选</a></td> 
    </tr>  
    <tr> 
        <td>虚拟背景</td> 
        <td>NIM_Web_VirtualBackground_vx.x.x.js</td> 
        <td>可选</a></td> 
    </tr>
    <tr> 
        <td>美颜</td> 
        <td>NIM_Web_AdvancedBeauty_vx.x.x.js</td> 
        <td>可选</a></td> 
    </tr> 
    <tr> 
        <td>AI 音效</td> 
        <td>NIM_Web_AIAudioEffects_vx.x.x.js</td> 
        <td>可选</a></td> 
    </tr>  
     <tr> 
        <td>啸叫检测</td> 
        <td>NIM_Web_AIhowling_vx.x.x.js</td> 
        <td>可选</a></td> 
    </tr>  
    </tbody>
    </table>

    - 若您集成的是 **v4.6.20 之前版本**的 NERTC SDK，解压 SDK 压缩包后，直接将 `/js` 目录下的 **NIM_Web_NERTC_vx.x.x.js** 文件拷贝到项目文件所在的目录下。

4. 在项目文件的 head 中引入 `NIM_Web_NERTC_vx.x.x.js` 等文件。其中，“x.x.x” 为 SDK 的版本号，请参考压缩包解压后的文件名修改。

5. 请在[支持的浏览器](https://doc.yunxin.163.com/nertc/quick-start/TU5NjUzNjU?platform=web#Web%E7%AB%AF%E6%94%AF%E6%8C%81%E5%93%AA%E4%BA%9B%E6%B5%8F%E8%A7%88%E5%99%A8%E7%B1%BB%E5%9E%8B%E5%92%8C%E7%89%88%E6%9C%AC%EF%BC%9F)上打开 `index.html` 文件，此时您可以看到基本的界面，代表 SDK 已导入完成。

## <span id="后续步骤">后续步骤</span>

<a href="https://doc.yunxin.163.com/nertc/quick-start/TA1NTk0NzY" target="_blank">实现音视频通话</a>