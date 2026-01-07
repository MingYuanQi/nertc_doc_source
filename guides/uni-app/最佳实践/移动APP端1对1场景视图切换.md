移动 App 中 1 对 1 业务场景相应的音视频通话界面包含 **本端视频窗口**、**远端视频窗口**，效果如下图所示：

<img alt="效果图.png" src="https://yx-web-nosdn.netease.im/common/627ad829c34530bfe2ee17b37811f500/android效果图.png" style="width:50%;border: 2px solid #f2f2f2;">

## <span id="视图窗口动态切换">视图窗口动态切换</span>

用户如果有需求，本端窗口和远端窗口允许动态切换，那么在调用 [`changeVideoCanvas`](https://doc.yunxin.163.com/nertc/references/uniapp/typedoc/Latest/zh/html/modules/nertc.nertc-1.html#changevideocanvas) 接口实现。

::: note notice
必须本端和远端的视图都渲染完成后，才可以调用该接口，否则不生效
:::

### 示例项目源码

网易云信提供 [视图切换示例项目源码（在 `1v1Call.nvue` 文件中）](https://yx-web-nosdn.netease.im/package/uniplugin-nertc-debug-demo_v5.6.0.zip?download=uniplugin-nertc-debug-demo_v5.6.0.zip)，您可以参考该源码。

```javascript
this.engine.changeVideoCanvas({
    localVideoCanvasConfig: { //本端试图的配置内容
        renderMode: 2, // 0 表示使用视频，视频等比缩放，1 表示适应区域，会裁剪，2：折中方案 //当前 demo 先使用数字，正式版本会是枚举
        mirrorMode: 2, //1 表示启用镜像，2 表示不启用
    },
    remoteVideoCanvasConfig: { //远端试图的配置内容
        renderMode: 1, // 0 表示使用视频，视频等比缩放，1 表示适应区域，会裁剪，2：折中方案 //当前 demo 先使用数字，用户可以使用枚举
        mirrorMode: 1, //1 表示启用镜像，2 表示不启用
        userID: this.remoteUserID,
        userStringID: this.remoteUserID + ''
    }
})
```