网易云信 AI 虚拟背景 SDK 基于高精度的 AI 人体检测和人像分割技术，自动识别场景中的人像，支持在各种复杂场景下精准实现人景分离和背景替换。本文档介绍如何基于网易云信 AI 虚拟背景 SDK（NERTC VirtualBackground SDK），在音视频场景中实现背景替换的效果。

接入网易云信 AI 虚拟背景 SDK 后，配合 NERTC SDK，应用可以自动识别通话和直播中的人像，并将参会人周围的环境进行虚化显示或替换为指定图像。

- 社交娱乐场景，主播在直播过程中，可以将虚拟背景设置为贴合直播主题的画面，提升观众的体验；此外，还能够对主播的隐私起到保护作用。
- 在线教育场景，老师可以将教学内容相关场景设为背景，提升课程的沉浸感。
- 智慧金融场景，虚拟背景能起到取代坐席背景墙的作用，有利于银行等金融机构的坐席人员在业务办理过程中规范通话背景以及节约传统背景板的成本。

## 注意事项
- 建议在背景相对简单、采光相对均匀的情况下使用此功能。
- 在背景要求较高的场景中，可以在通话用户的身后搭建绿幕，以获得更好的虚拟背景使用体验。
- 虚拟背景 SDK 不支持同时实现背景虚化和背景替换效果。

## 项目结构

 AI 虚拟背景 SDK 的主要文件位于 `/3rdparty/matting_service/mac` 中，其中主要项目文件如下：

| 文件夹                 | 说明                    |
| ------------------------- | ------------------------- |
| matting_service.framework | 背景替换 framework 所在路径 |
| MNN.framework             | 依赖的 framework        |
| opencv2.framework         | 依赖的 framework        |

## 环境准备
- Xcode 11 及以上版本。
- mac OSX 10.11 或以上版本的 Mac 设备。
- 运行示例项目需要使用 OpenSSL。

## 集成 SDK
1. 在<a href="https://yunxin.163.com/">官网首页</a>通过微信、在线消息或电话等方式联系云信商务经理获取 AI 虚拟背景 SDK。
2. 将库文件拷贝到本地目录 `3rdparty\matting_service\`。
3. 在工程文件中导入库。

    例如在 QT Creator 中导入库：
    ```
    DEFINES += USE_AILAB

    INCLUDEPATH +=  $$PWD/3rdparty/matting_service/mac/matting_service.framework/Headers \
                            $$PWD/3rdparty/matting_service/mac/MNN.framework/Headers \
                            $$PWD/3rdparty/matting_service/mac/opencv2.framework/Headers

            LIBS += -F$$PWD/3rdparty/matting_service/mac -framework matting_service -framework matting_service \
                    -F$$PWD/3rdparty/matting_service/mac -framework MNN -framework MNN \
                    -F$$PWD/3rdparty/matting_service/mac -framework opencv2 -framework opencv2 \
                    -framework Accelerate
    ```
### 实现步骤

1. 初始化 `matting_service`。
    ```
    MattingInit(nullptr);

    // 使用背景图片，最好使用32bit对齐的背景图 （二选一）
    QImage img(":/image/matting_bg2.png");
    img.convertTo(QImage::Format_RGB888);
    MattingSetBackground(img.bits(), img.height(), img.width(), MATTING_RGB, 0, false);

    // 或者使用背景虚化 （二选一）
    MattingActivateBlurMode(0);
    ```
2. 调用 [`setParameters`](https://doc.yunxin.163.com/nertc/api-refer/macOS/doxygen/Latest/zh/html/classnertc_1_1_i_rtc_engine_ex.html#a4d39a81643f19979461ec0bb569f4a0d) 接口，将 `kNERtcKeyEnableVideoCaptureObserver` 的值设置为 `YES`，开启摄像头采集数据的回调。

::: note note
- V5.3.0 及之后版本需要执行该步骤。
- V4.6.X 版本请忽略该步骤。
:::
3. 在图像数据回调中调用 `matting_service`。
    ```
    // IRtcEngineEventHandlerEx::onCaptureVideoFrame 回调接口的处理函数
    void NEEventHandler::mattingService(void* data,
                        NERtcVideoType /*type*/,
                        uint32_t width,
                        uint32_t height,
                        uint32_t count,
                        uint32_t /*offset*/[kNERtcMaxPlaneCount],
                        uint32_t /*stride*/[kNERtcMaxPlaneCount],
                        NERtcVideoRotation /*rotation*/)
    {
        if (width == 0 || height == 0) {
            return;
        }

        if (width != m_oldSize.width() || height != m_oldSize.height()) {
            m_oldSize.setWidth(width);
            m_oldSize.setHeight(height);
            m_mask = new unsigned char[width * height * count];
            memset(m_mask, 0, width * height * count);
        }

        MattingPredict((unsigned char*)data, m_mask, height, width, MATTING_I420, 0, false);
    }
    ```
3. （可选）如果使用 Qt Creator，为保证项目最终的执行文件可以运行，需要添加编译后处理命令。
     1. 打开Qt Creator，点击左侧的 `Projects` 标签，切换到当前项目的 `Build & Run` 项。
     2. 在 `Build Steps` 项目下，点击 `Add Build Step`。
     3. 添加后处理命令。

        最终的效果如下：

        ![](https://yx-web-nosdn.netease.im/quickhtml%2Fassets%2Fyunxin%2Fdoc%2FG2-VirtualBackground-mac01.png)

### API 时序

![](https://yx-web-nosdn.netease.im/quickhtml%2Fassets%2Fyunxin%2Fdoc%2FG2-VirtualBackground-Win03.jpg)

## API 参考

| API                      | 说明                        |
| ------------------------ | ----------------------------- |
| [MattingInit](https://dev.yunxin.163.com/docs/interface/NERTC_VirtualBackground_SDK/Latest/PC/html/matting__service_8hpp.html#a774b69da08942576185c794acef96f90)              | SDK 初始化。              |
| [MattingReset](https://dev.yunxin.163.com/docs/interface/NERTC_VirtualBackground_SDK/Latest/PC/html/matting__service_8hpp.html#ad75db1db704e1a5202ec2d701e72f0d1)             | AI 虚拟背景功能状态重置。  |
| [MattingSetBackground](https://dev.yunxin.163.com/docs/interface/NERTC_VirtualBackground_SDK/Latest/PC/html/matting__service_8hpp.html#adaed1b1fb31d2fcf3a2272ba35202655)    | 设置自定义背景。      |
| [MattingActivateBlurMode](https://dev.yunxin.163.com/docs/interface/NERTC_VirtualBackground_SDK/Latest/PC/html/matting__service_8hpp.html#a902ead4a62f75f289269878f781d72c3)  | 激活背景虚化模式。   |
| [MattingPredict](https://dev.yunxin.163.com/docs/interface/NERTC_VirtualBackground_SDK/Latest/PC/html/matting__service_8hpp.html#a704e1ba2c63f5516a1468b8a4b8c0a4f)           | 开始 AI 背景虚化或背景替换。  |
