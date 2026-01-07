<!-- keywords: 实时音视频，音视频通话，webrtc，RTC，跑通示例项目 -->

您可以通过跑通 Sample Code，体验网易云信音视频相关功能，包括音频通话、视频通话和进阶功能。

## <span id="开发环境">开发环境</span>
请确认您的开发环境满足以下要求

- Xcode 10.0 或以上版本。
- iOS 9.0 或以上版本且支持音视频的 iOS 设备。
  ::: note notice
  由于模拟器缺少摄像头及麦克风能力，因此工程需要在真机运行，请确保已正确连接 iOS 设备。
  :::
- iOS 设备和您的开发电脑已经连接到网络。

## <span id="前提条件">前提条件</span>
请确认您已完成以下操作：

- <a href="https://doc.yunxin.163.com/console/guide/TIzMDE4NTA?platform=console" target="_blank">创建应用并获取 App Key</a>。
- <a href="https://doc.yunxin.163.com/nertc/quick-start/TYzODcyNjE" target="_blank">开通音视频通话 2.0 服务</a>。


## <span id="快速跑通Sample Code">快速跑通Sample Code</span>

::: note note
在运行示例项目之前，请在云信控制台中为指定应用[开通调试模式](https://doc.yunxin.163.com/nertc/quick-start/TQ0MTI2ODQ?platform=android#修改鉴权方式)。调试模式建议只在集成开发阶段使用，请在应用正式上线前改回安全模式。
:::

1. 下载[NERTC API 示例代码](https://github.com/netease-im/G2-API-Examples/tree/main/ios)至您的本地工程。

    Podfile 文件中包括以下内容：

    ```objc
    # Uncomment the next line to define a global platform for your project
    # platform :ios, '9.0'

    target 'NERTC-API-Example-OC' do
      # Comment the next line if you don't want to use dynamic frameworks
      use_frameworks!
      
      # Pods for NERtc_ios
      pod 'NERtcSDK', '4.6.20'
      pod 'SSZipArchive'
      pod 'Masonry'
    end
    ```
    您可以通过修改pod 'NERtcSDK'后的sdk版本号，使用不同版本的sdk

2. cd 到 iOS 目录下，执行如下命令，下载云信 SDK 和其他第三方库。
    ```
    pod install
    ```
3. 通过`pod`集成后，双击 `NERTC-API-Example-OC.xcworkspace`，通过 Xcode 打开工程。
4. 在 `Debug\NTESAppConfig.h` 文件中填入您的 AppKey。
    ```
    #ifndef NTESAppConfig_h
    #define NTESAppConfig_h

    #define AppKey @"<#请输入AppKey#>"

    #endif /* NTESAppConfig_h */
    ```
5. （可选）登录 Apple 开发者账号。
::: details 您可以参考此步骤登录账号，若已经登录，请忽略该步骤。
  1. 打开 Xcode，依次选择左上角菜单的 **Xcode** > **Preferences**。

  ![xcode_preference.jpg](https://yx-web-nosdn.netease.im/common/617f40359c91c084fbf86d34aeb0b9f7/xcode_preference.jpg)


  2. 依次单击 **Accounts** > 左下角的 **+** > **Apple ID** > **Continue**。

  ![xcode_account.jpg](https://yx-web-nosdn.netease.im/common/aff66bf004426c55e385316dc5b8413a/xcode_account.jpg)
  
  3. 输入 Apple ID 和 Password 登录。
  
  ![xcode_login_app_id.jpg](https://yx-web-nosdn.netease.im/common/fc7b4464113da6dd8939a74a75956b40/xcode_login_app_id.jpg)

:::
6. [设置签名并添加媒体设备权限](https://doc.yunxin.163.com/nertc/quick-start/TM5NzI5MjI?platform=iOS#%E8%AE%BE%E7%BD%AE%E7%AD%BE%E5%90%8D%E5%B9%B6%E6%B7%BB%E5%8A%A0%E5%AA%92%E4%BD%93%E8%AE%BE%E5%A4%87%E6%9D%83%E9%99%90)。
7. 运行工程。
    1. 将 iOS 设备连接到开发电脑，单击 Xcode 上方的的 **Any iOS Device**，在弹出的选项框选择该 iOS 设备。 

 
    ![xcode_select_device_new.png.jpg](https://yx-web-nosdn.netease.im/common/5bf7b4e6c678580f65a58ca9c2b39834/xcode_select_device_new.png.jpg)

    ![xcode_select_real_device_new.jpg](https://yx-web-nosdn.netease.im/common/92e2c3adaf68ada15c6058a8fc5869bb/xcode_select_real_device_new.jpg)

     
    2. 单击 **Build** 按钮编译和运行示例源码。


    ![xcode_build.jpg](https://yx-web-nosdn.netease.im/common/7c4635d1c30e6636a706cb668c41804c/xcode_build.jpg)


    3. 运行成功后，您可以单击体验音视频相关功能。

