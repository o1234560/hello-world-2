# 使用expo创建项目

## 介绍

Expo 是一个 React Native 框架，使开发 Android 和 iOS 应用更容易。我们的框架提供基于文件的路由、原生模块的标准库，以及更多功能。Expo 是开源的，并且在 [GitHub](https://github.com/expo/expo) 和 [Discord](https://chat.expo.dev/) 上有活跃的社区。

## 系统要求

- [Node.js（长期支持版）](https://nodejs.cn/)。
- 支持 macOS、Windows（Powershell 和 [WSL 2](https://expo.fyi/wsl)）以及 Linux。

我们建议从 `create-expo-app` 创建的默认项目开始。默认项目包含示例代码，帮助你快速入门。

```powershell
npx create-expo-app@latest --template default@sdk-55
```

注意： 在 SDK 55 过渡期间，不带 `--template` 标志的 `create-expo-app@latest` 会创建一个 SDK 54 项目。如果你计划在实体设备上使用 Expo Go，请使用 SDK 54 项目。否则，使用 `--template default@sdk-55` 创建一个 SDK 55 项目。你也可以通过添加 [`--template` 选项](https://expo.nodejs.cn/more/create-expo/#--template) 来选择不同的模板。

## 设置环境

- 安装 [Java SE 开发工具包 (JDK)](https://openjdk.org/)

- 安装 [Android Studio](https://developer.android.google.cn/studio?hl=zh-cn)，选择标准安装
  
  - 安装完成后，打开 Android Studio，进入 **设置** > **语言与框架** > **Android SDK**。在**SDK平台**选择**Android SDK Platform 35** 和 **Source for Android 35**。
  
  - 点击 **SDK Tools** 标签，并确保你至少安装了一个版本的 **Android SDK Build-Tools** 和 **Android Emulator**。
  
  - 工具安装完成后，配置 `ANDROID_HOME` 环境变量。进入 **Windows 控制面板** > **用户账户** > **用户账户**（再次点击）> **更改我的环境变量**，然后点击 **新建** 来创建一个新的 `ANDROID_HOME` 用户变量。该变量的值应指向你的 **Android SDK 路径:**`c:\users\username\AppData\Android\Sdk`
  
  - 要将 platform-tools 添加到路径中，请转到 **Windows 控制面板** > **用户账户** > **用户账户**（再次）> **更改我的环境变量** > **Path** > **编辑** > **新建**，然后将 **platform-tools** 的路径添加到列表中`c:\users\usersname\AppData\Local\Android\Sdk\platform-tools`
  
  - 最后，确保你可以从 PowerShell 运行 `adb`。例如，运行 `adb --version` 来查看你的系统正在运行哪个版本的 `adb`。

## 在安卓设备上运行你的应用

1. **安装 expo-dev-client**

```powershell
npx expo install expo-dev-client
```

2. **启用 USB 调试**

默认情况下，大多数安卓设备只能安装和运行从 Google Play 下载的应用。在开发过程中，需要在设备上启用 USB 调试才能安装你的应用。

要在你的设备上启用 USB 调试，启用“开发者选项”模式。然后，你可以返回 设置 > 开发者选项 来启用“USB 调试”。

3. **通过 USB 连接你的设备**

通过 USB 将你的 Android 设备连接到电脑。

通过在终端中运行 `adb devices` 检查你的设备是否正确连接到 ADB（Android 调试桥）。你应看到你的设备列出，并在其旁边显示 `device`。例如：

```powershell
> adb devices

List of devices attached
8AHX0T32K device
```

4. **运行你的应用**

```powershell
npx expo run:android
```

该命令会在构建应用后运行开发服务器。你可以在下一页跳过运行 `npx expo start`。

## 开始开发

当使用 **expo go** 或者是 **已经构建的应用** 进行开发时，开发步骤如下：

1. **启动开发服务器**
   
   ```powershell
   npx expo start
   ```

2. **在设备或模拟器上打开应用**
   
   运行上述命令后，根据提示打开应用，进行开发。    
   
   修改代码后，会自动更新应用。

> **注意：**
> 
> 当只修改js、jsx、ts、tsx时，会自动更新应用，无需重新构建。
> 
> 当修改原生或相关配置时，需要进行重新构建。
