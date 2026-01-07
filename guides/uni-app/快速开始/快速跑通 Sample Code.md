本文介绍如何快速跑通 RTC uni-app 的 Demo。

## 前提条件

在开始运行示例项目之前，请确保您已完成以下操作：
- <a href="https://doc.yunxin.163.com/console/guide/TIzMDE4NTA?platform=console" target="_blank">已创建应用并获取 App Key</a>
- <a href="https://doc.yunxin.163.com/docs/TA3ODAzNjE/DcyNzA2NTA?platformId=50612" target="_blank">已开通音视频通话2.0</a>

## 开发环境要求

在运行示例项目之前，请确保开发环境满足以下要求：

环境要求 |说明 |
---- | -------------- | ---------
[HBuilderX](https://www.dcloud.io/hbuilderx.html) | 3.0.0 或以上版本。 | 
iOS 及 Android 设备 | App 运行环境满足如下要求：|\
 | - iOS 11.0 或以上版本的 iOS 设备。暂不支持模拟器。 |\
 | - Android 系统 5.0 或以上版本的 Android 设备。暂不支持模拟器。 | 


## 注意事项

- 当前 RTC uni-app 支持 Android、iOS 以及 H5 平台。

- 示例项目需要在 **RTC 调试模式**下使用，此时无需传入 Token。修改鉴权方式的方法请参见 <a href="https://doc.yunxin.163.com/nertc/quick-start/TQ0MTI2ODQ?platform=android" target="_blank">Token 鉴权</a> 。

- 您可以在集成开发阶段使用调试模式进行应用开发与测试。但是出于安全考虑，应用正式上线前，请在控制台中将指定应用的鉴权方式改回安全模式。


## 示例源码目录结构说明
```
├── App.vue
├── common
│   └── nertc-nvue.css        # nertc 定义的 css 样式
└── NERtcUniappSDK-JS         # nertc uniapp sdk js 封装层
│       ├── lib                          # js 库文件
│       └── nertc-view                   # 原生渲染 view   
├── main.js
├── manifest.json
├── nativeplugins                        
│   └── NERTCUniPluginSDK        # 本地依赖的 nertc音视频SDK 原生插件
│       ├── android
│       ├── ios
│       └── package.json
├── package.json
├── pages                                # demo 源码
│   ├── sample
│   │   ├── 1v1Call                      # 音视频点对点通话demo
│   │   ├── multiCall                    # 音视频多人通话demo
│   │   ├── setConfig                    # 音视频通话初始化参数配置页面
│   ├── index
│   │   └── index.vue                    # demo的首页入口而已
│   ├── keyCenter                        # demo参数配置中心
├── pages.json                           # 显示的所有页面
├── static                               # 资源文件
│   ├── new_file.json
│   └── logo.png
├── uni.scss
└── unpackage                            # uniapp 打包生成的文件
```  
## 步骤1：运行示例源码 

::: note important
示例源码仅供开发者接入参考，实际应用开发场景中，请结合具体业务需求修改使用。

若您计划将源码用于生产环境，请确保应用正式上线前已经过全面测试，以免因兼容性等问题造成损失。
:::

1. 下载 [Demo 源码](https://doc.yunxin.163.com/nertc/quick-start/zUyNzEwOTQ?platform=uniapp#demo-源码)至您本地。

    ::: note note
    本示例项目源码基于 VUE 2.0 开发。
    :::

  
2. 使用 HBuilderX 打开示例项目源码。
3. 示例源码中缺少 SDK 初始化所需的 appKey，以及 token，需要在 `pages/KeyCenter.js` 文件中配置应用的 App Key。


![image.png](https://yx-web-nosdn.netease.im/common/630b6b1000745be482766879290c9cdc/image.png)


::: note note
为了方便运行 Demo，您可以在本地跑通和开发集成阶段使用**RTC 调试模式**，此时无需传入 Token。修改鉴权方式的方法请参见 <a href="https://doc.yunxin.163.com/nertc/quick-start/TQ0MTI2ODQ?platform=android" target="_blank">Token 鉴权</a>，控制台上默认为安全模式，此时需要传入 Token 的值 。

:::


4. 打开项目根目录下的 `manifest.json` 文件，在基础配置页面，单击 **uni-app应用标识（AppID）** 文本框右侧的**自动获取**，如下图所示。

    详细的参数说明请参见 [manifest.json 应用配置](https://zh.uniapp.dcloud.io/collocation/manifest.html)。

    ![image1.png](https://yx-web-nosdn.netease.im/common/e741187b62b4fea15e2372c03276bf74/image1.png)


5. 在项目中导入插件


    ![POPO-screenshot-20230713-234522.png](https://yx-web-nosdn.netease.im/common/7cfe4eead3a2e5f6a4b5fba534572709/POPO-screenshot-20230713-234522.png)


## 步骤2：制作自定义调试基座

您需要使用自定义基座打包 uni-app 插件，以便在手机上运行调试，并且可以在 HBuilder 控制台看到日志。

::: note note
uni-app 官方自定义调试基座使用说明请参考 [什么是自定义调试基座及使用说明](https://uniapp.dcloud.net.cn/tutorial/run/run-app.html#customplayground)。
:::

1. 在 HBuilderX 的顶部菜单栏中选择 **运行** > **运行到手机或模拟器** > **制作自定义调试基座**。 


    ![image2.png](https://yx-web-nosdn.netease.im/common/54bf0e88899b3f815e1641e97dc647bd/image2.png)



2. 在弹出的对话框中选择**打自定义调试基座**，单击**打包**。详细步骤请参见 [uni-app 官网的自定义基座](https://uniapp.dcloud.net.cn/tutorial/run/run-app.html#customplayground)。

    
    ![image3.png](https://yx-web-nosdn.netease.im/common/4722b3dd8ce646cd6ad5226e1eaa9aff/image3.png)



    打包成功后，控制台会收到 uni-app 的相关提示。


    ![image4.png](https://yx-web-nosdn.netease.im/common/1f72e0eaa1415504daf84194ca56a34a/image4.png)



3. 使用自定义调试基座。

    在 HBuilderX 的顶部菜单栏中选择**运行** > **运行到手机或模拟器** > **运行基座选择**  > **自定义调试基座**。


    ![image5.png](https://yx-web-nosdn.netease.im/common/2ec4a5abf0b253b68a98bd89be46327c/image5.png)



## 步骤3：运行代码

选择**运行** > **运行到手机或模拟器**，选择自己的设备，并运行。

![image6.png](https://yx-web-nosdn.netease.im/common/f5c232f0211245694da58f79dff15d98/image6.png)


  
::: note note
在真机中运行项目，运行成功后，为方便体验，您可以集成一个 Web 端 SDK 或者其他平台的 SDK，输入相同的 AppKey 和 channelName 即可进入同一个房间进行音视频通话。
:::
