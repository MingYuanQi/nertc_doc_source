本文为您介绍了 Unreal Engine 集成 SDK 到您自己的unreal工程中的操作步骤，帮助您快速集成 SDK 并实现实时音视频通话的基本功能。
## 前提条件
在开始运行工程之前，请您准备以下开发环境：
- [Unreal Engine](https://www.unrealengine.com/zh-CN/download) 4.25 或以上版本。
    - [下载Epic Games启动程序](https://www.unrealengine.com/zh-CN/download)
    - [安装并启动虚幻引擎](https://docs.unrealengine.com/5.2/zh-CN/installing-unreal-engine/)
- 操作系统与开发环境要求：

:::::: div custom-tabs
::: tab Android

- Android SDK API 等级 19 或以上。
- Android Studio 3.0 或以上版本。
- Android 系统 4.4 或以上版本的移动设备。

:::
::: tab iOS
- Xcode 10 及以上版本。
- iOS 9.0 及以上版本的 iOS 设备。
:::
::: tab Windows
- 开发环境：Microsoft Visual Studio 2017（推荐）或以上版本
- 操作系统：Microsoft Windows 7 或以上版本
- 编译器：Microsoft Visual C++ 2017 或以上版本
:::
::: tab macOS
- Xcode 11 及以上版本。
- mac OS X 10.11 或以上版本的 Mac 设备。
- 项目已配置有效的开发者签名。
:::
::::::


## 步骤1 集成 SDK


### 1. （可选）新建项目


::: details 介绍如何新建项目，如果集成到已有的项目，请忽略该步骤。
打开 Unreal Project Browser，选择行业类别、项目模板、实现方式选择 **C++**，选择项目的存储路径和项目名称，单击**创建**。


![新建UE项目.png](https://yx-web-nosdn.netease.im/common/eebe52c4772bfb364907b3cb82b8c123/新建UE项目.png)

详细的操作步骤请参见[新建项目](https://docs.unrealengine.com/5.2/zh-CN/creating-a-new-project-in-unreal-engine/)。
 
:::

### 2. 导入 SDK

1. 在<a href="https://doc.yunxin.163.com/nertc/sdk-download?platform=UE" target="_blank">云信 SDK 下载中心</a>获取当前最新版本的 NERTC SDK。
2. （可选）在工程的根目录（xxxx.uproject文件所在目录）下新建 **Plugins** 文件夹，如果已存在该文件夹，则忽略该步骤。
3. 将解压缩后的 NERTC SDK 的 `NertcPlugin` 拷贝至 **Plugins** 文件夹。
4. 添加云信依赖库。

    在 `Project/Source/Project/Project.Build.cs` 文件中，向 `PublicDependencyModuleNames.AddRange()` 添加云信依赖库。
    ```
    PublicDependencyModuleNames.AddRange(new string[] { "Core", "CoreUObject", "Engine", "InputCore", "NertcPlugin"});
    ```
5. 编译工程。

    1. 刷新 C++工程（Visual studio/Xcode），打开 C++工程，可以看到类似下图的目录结构，然后编译游戏。

    
        ![Snipaste_2023-09-20_16-09-34.png](https://yx-web-nosdn.netease.im/common/3545db00384ee69b84a819546d40497e/Snipaste_2023-09-20_16-09-34.png)


    2. 编译完成后重新启动 Unreal Engine Editor，在菜单栏中选择**编辑** > **插件** ，打开插件管理器，点击**Other**即可以看到 NERTC SDK 已经引入工程了，确定 NERTC SDK 是 Enabled 状态。


        ![Snipaste_2023-09-20_16-14-23.png](https://yx-web-nosdn.netease.im/common/2dcd9c4fd84730155a4470dcba527710/Snipaste_2023-09-20_16-14-23.png)

### 3. 设置内存分配方式

设置 Unreal Engine 在 iOS、macOS 平台上使用标准 C 的内存分配方式，避免在使用 SDK 插件时，出现异常的内存释放问题。

打开项目目录的 `Source/example/example.Target.cs` 文件，在构造函数中添加如下代码：

```
//强制指定使用c语言风格的内存分配方式
if (Target.Platform == UnrealTargetPlatform.Mac || Target.Platform == UnrealTargetPlatform.IOS) 
{
    GlobalDefinitions.Add("FORCE_ANSI_ALLOCATOR=1");
}

//关闭UE内置的crash收集
GlobalDefinitions.Add("NOINITCRASHREPORTER=1");
```