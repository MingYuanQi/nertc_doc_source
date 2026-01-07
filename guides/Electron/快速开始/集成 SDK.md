本文为您介绍了在 Electron 端集成 NERTC SDK 的操作步骤，帮助您快速集成 SDK 并实现音视频通话的基本功能。

## 开发环境

在开始运行工程之前，请您准备以下开发环境：

- Node.js 10.12.0 及以上版本， 推荐使用 LTS 版本。
- Electron 5+ 版本，推荐使用 [Electron](https://www.electronjs.org/releases/stable) 最新发布的四种版本。
- Windows 7 及以上版本的操作系统，macOS 10.14.6 及以上版本的操作系统。暂不支持 Linux 桌面版。

## 集成 Electron SDK

NERTC Electron SDK 需要通过 npm 方式集成到您的项目中，具体操作步骤如下。

1. 在项目文件路径下，运行如下命令进行安装 NERTC SDK for Electron。

```
npm install nertc-electron-sdk
```

2. 您可以通过以下两种方式将 SDK 引入至您的项目中。

```
//方式1
import NERtcEngine from 'nertc-electron-sdk'
//方式2
var NERtcSDK = require('nertc-electron-sdk').default
var nertcEngine = new NERtcSDK.NERtcEngine()
```

## 后续步骤

[实现音视频通话](https://doc.yunxin.163.com/nertc/quick-start/zMzNjA2ODg?platformId=50456)

## 常见问题

- [我运行项目时，Electron 控制台为什么会提示 `xxx is not defined`？](https://doc.yunxin.163.com/nertc/quick-start/jU4NTEwNzg?platformId=50456#1)
- [我使用 vue create 创建项目，但在运行 vscode 时提示 `Module parse failed: Unexpected character '�' (1:2)`，应该如何解决？](https://doc.yunxin.163.com/nertc/quick-start/jU4NTEwNzg?platformId=50456#2)
- [在 Mac 设备上运行 Electron 应用时，我的本地预览画面为什么无法正常显示？](https://doc.yunxin.163.com/nertc/quick-start/jU4NTEwNzg?platformId=50456#3)
- [在 Windows 设备上运行 Electron 应用时，Electron 控制台提示 `Error:The specified module could not be found`，应该如何解决？](https://doc.yunxin.163.com/nertc/quick-start/jU4NTEwNzg?platformId=50456#4)
- [我用 electron-builder 打包应用时，Electron 控制台提示 `Uncaught Error: The specified module could not be found ...\nertc-electron-sdk.node`，应该如何解决？](https://doc.yunxin.163.com/nertc/quick-start/jU4NTEwNzg?platformId=50456#5)
- [如何使用 Mac M1 arm64 架构设备集成 NERTC Electron SDK？](https://doc.yunxin.163.com/nertc/quick-start/jU4NTEwNzg?platformId=50456#6)
