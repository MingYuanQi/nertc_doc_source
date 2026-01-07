本文介绍如何在 uni-app 框架中集成 NERTC SDK。
## 开发环境要求

在开始集成 NERTC SDK 之前，请确保开发环境满足以下要求：

环境要求 |说明 |
---- | -------------- | ---------
[HBuilderX](https://www.dcloud.io/hbuilderx.html) | 3.0.0 或以上版本。 | 
iOS 及 Android 设备 | App 运行环境满足如下要求：|\
 | - iOS 11.0 或以上版本的 iOS 设备。 |\
 | - Android 系统 5.0 或以上版本的 Android 设备。 | 

  
## 集成 SDK 
### 步骤1：（可选）创建 uni-app 项目
::: details 在 HBuilderX 中创建一个新的 uni-app 项目，若是需要集成到已有的项目中，请忽略该步骤。
详细的操作步骤请参见[快速上手 uni-app](https://uniapp.dcloud.net.cn/quickstart-hx.html)

1. 在 HBuilderX 的顶部菜单栏中选择 **文件** > **新建** > **项目**。
2. 在弹出的对话框中，项目类型选择 **uni-app** ，输入工程名，选择模板，单击**创建**。

![创建uniapp项目.png](https://yx-web-nosdn.netease.im/common/4ab66e42f5e10f2a6f105f3f09feffb5/创建uniapp项目.png)
:::
### 步骤2：集成 SDK


1. 前往 [SDK 下载页面](https://doc.yunxin.163.com/nertc/sdk-download?platform=uniapp)获取当前最新版本 SDK，包括**JS 封装 SDK** 和 **uni-app 插件**两个包。

  
2. 将解压缩后的 SDK 文件复制到 uni-app 项目的根目录下的 `nativeplugins` 文件夹中。如果不存在该文件夹，请手动创建。

![POPO-screenshot-20230714-000436.png](https://yx-web-nosdn.netease.im/common/a53d0e06323413115733745155afe617/POPO-screenshot-20230714-000436.png)

<!--
**方式2：通过 uni-app SDK 插件市场集成**

NERTC SDK 已发布到 uni-app SDK 插件市场。


1. 登录 [uni-app 插件市场]()，在插件详情页中选择其中一种方式导入 SDK：

    - 购买（0元） for 云打包，选择相应的项目导入。

    - 下载 for 离线打包，离线导入。

        下载 SDK 到本地，解压缩 `NERTCUniPluginSDK `文件。

2. 将解压缩后的文件夹，直接复制到项目工程根目录下的 `nativeplugins` 文件夹中。如果不存在该文件夹，请手动创建。

-->

### 步骤3：在 uni-app 项目中导入插件
1. 单击项目目录的 **manifest.json** 文件后，单击 **App 原生插件配置** 中的**选择本地插件**或**选择云端插件**。

    ![POPO-screenshot-20230713-234522.png](https://yx-web-nosdn.netease.im/common/7cfe4eead3a2e5f6a4b5fba534572709/POPO-screenshot-20230713-234522.png)

2. 在弹出的选择框中，选择 **NERTC 音视频 SDK** 后，单击**确认**，即添加成功。

    ![POPO-screenshot-20230714-001131.png](https://yx-web-nosdn.netease.im/common/43032bcaba14f89ba0fb3672e5b79ef3/POPO-screenshot-20230714-001131.png)


### 步骤4： 使用自定义基座打包 uni-app 插件

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



### 步骤5：运行代码

选择**运行** > **运行到手机或模拟器**，选择自己的设备，并运行。

![image6.png](https://yx-web-nosdn.netease.im/common/f5c232f0211245694da58f79dff15d98/image6.png)


  
::: note note
在真机中运行项目，运行成功后，为方便体验，您可以集成一个 Web 端 SDK 或者其他平台的 SDK，输入相同的 AppKey 和 channelName 即可进入同一个房间进行音视频通话。
:::


## 集成 JS 封装层


1. 导入 JS 封装层。

    在插件市场的 [NERtcUniappSDK 音视频插件（JS）](todo) 界面，单击右侧的**使用 HBuilderX 导入插件**。

2. 导入的 JS 封装层将存储在 `components` 目录中。


    ![POPO-screenshot-20230713-235507.png](https://yx-web-nosdn.netease.im/common/b142111c2aa59e5b86e0249f1867cd17/POPO-screenshot-20230713-235507.png)


3. 导入后，可以在业务代码中引入 JS 封装层，并调用 Express 相关接口，示例如下：
    ```

    import NERTC from "@/NERtcUniappSDK-JS/lib/index";
    import NertcLocalView from "@/NERtcUniappSDK-JS/nertc-view/NertcLocalView";
    import NertcRemoteView from "@/NERtcUniappSDK-JS/nertc-view/NertcRemoteView";

    ```