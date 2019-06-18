---
title: flutter安装使用遇到的问题
date: 2019-06-18 14:10:34
categories: flutter
tags: flutter
---

按照https://flutter.dev/docs/get-started/test-drive?tab=vscode依次安装使用，过程中遇到的问题：

### 1.执行 flutter doctor 出现：

```bash
[!] Android toolchain - develop for Android devices (Android SDK version 29.0.0)
    ✗ Android license status unknown.
      Try re-installing or updating your Android SDK Manager.
      See https://developer.android.com/studio/#downloads or visit
      https://flutter.dev/setup/#android-setup for detailed instructions.
```

解决办法：

执行： flutter doctor --android-licenses

可能出现的问题：

A newer version of the Android SDK is required. To update, run:
/Users/xxxx/Library/Android/sdk/tools/bin/sdkmanager --update

执行：
/Users/xxxx/Library/Android/sdk/tools/bin/sdkmanager --update

出现问题：

```bash
Exception in thread "main" java.lang.NoClassDefFoundError: javax/xml/bind/annotation/XmlSchema
	at com.android.repository.api.SchemaModule$SchemaModuleVersion.<init>(SchemaModule.java:156)
	at com.android.repository.api.SchemaModule.<init>(SchemaModule.java:75)
	at com.android.sdklib.repository.AndroidSdkHandler.<clinit>(AndroidSdkHandler.java:81)
	at com.android.sdklib.tool.sdkmanager.SdkManagerCli.main(SdkManagerCli.java:73)
	at com.android.sdklib.tool.sdkmanager.SdkManagerCli.main(SdkManagerCli.java:48)
Caused by: java.lang.ClassNotFoundException: javax.xml.bind.annotation.XmlSchema
	at java.base/jdk.internal.loader.BuiltinClassLoader.loadClass(BuiltinClassLoader.java:582)
	at java.base/jdk.internal.loader.ClassLoaders$AppClassLoader.loadClass(ClassLoaders.java:190)
	at java.base/java.lang.ClassLoader.loadClass(ClassLoader.java:499)
	... 5 more
```

这个问题是因为 java jdk 版本导致的，
解决办法：修改 java jdk 为 jdk1.8.0_201.jdk。

```bash
brew cask uninstall java# 卸載 java9
brew tap caskroom/versions
brew cask install java8# 安裝 java8
沒有這裡文件的touch ~/.android/repositories.cfg #，將在下一步中出現錯誤
brew cask install android-sdk
```

记得修改 JAVA_HOMRE 环境变量

然后一次执行：

```bash
/Users/xxxx/Library/Android/sdk/tools/bin/sdkmanager --update
flutter doctor --android-licenses // 提示y/n都选择y
```

再次执行

```bash
flutter doctor
```

出现成功提示

```bash
[✓] Android toolchain - develop for Android devices (Android SDK version 29.0.0)
```

### 2.执行 flutter doctor 出现：

```bash
✗ Xcode installation is incomplete; a full installation is necessary for iOS development.
      Download at: https://developer.apple.com/xcode/download/
      Or install Xcode via the App Store.
      Once installed, run:
        sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
```

解决办法：

打开 xcode -> 首选项(preference) -> 位置(locations)

选择 command line tools 的值原来是空的，选择 xcode11.0(安装的版本)

然后执行 flutter doctor

```bash
[!] iOS toolchain - develop for iOS devices (Xcode 11.0)
    ✗ libimobiledevice and ideviceinstaller are not installed. To install with Brew,
      run:
        brew update
        brew install --HEAD usbmuxd
        brew link usbmuxd
        brew install --HEAD libimobiledevice
        brew install ideviceinstaller
    ✗ ios-deploy not installed. To install:
        brew install ios-deploy
    ✗ CocoaPods not installed.
        CocoaPods is used to retrieve the iOS platform side's plugin code that responds to
        your plugin usage on the Dart side.
        Without resolving iOS dependencies with CocoaPods, plugins will not work on iOS.
        For more info, see https://flutter.dev/platform-plugins
      To install:
        brew install cocoapods
        pod setup

```

按提示进行安装就行了。

```bash
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel stable, v1.5.4-hotfix.2, on Mac OS X 10.14.5 18F132, locale
    zh-Hans-CN)

[✓] Android toolchain - develop for Android devices (Android SDK version 29.0.0)
[✓] iOS toolchain - develop for iOS devices (Xcode 11.0)
[✓] Android Studio (version 3.4)
[✓] Connected device (1 available)

• No issues found!
```
