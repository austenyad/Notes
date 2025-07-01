# Flutter & 鸿蒙 调研



参考地址文档：

[环境搭建1](https://gitcode.com/openharmony-tpc/flutter_samples/blob/master/ohos/docs/03_environment/openHarmony-flutter%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA%E6%8C%87%E5%AF%BC.md)

[环境搭建2](https://juejin.cn/post/7384992816907042825)



### 一、配置环境 & Run The OHos-Flutter Demo

#### 1. 下载并配置 ohos-flutter，并切换版本 ohos-flutter 3.22

**注意**：使用 ohos-flutter ，删除 google-flutter

```shell
git clone https://gitcode.com/openharmony-tpc/flutter_flutter/tree/3.22.0-ohos
```

下载项目到本地后，鸿蒙 Flutter 有两个分支 master 和 dev 可用供开发者使用，dev不断在更新相比master拥有更多功能。

```shell
// 3.7.12
git checkout -b dev origin/dev 
// 3.22.0 -- 选择最新的
git checkout -b 3.22.0-ohos origin/3.22.0-ohos
```

#### 2. 下载最新的 DevEco-Studio : `DevEco Studio 5.0.5 Release`

https://developer.huawei.com/consumer/cn/download/

#### 3. Java 环境：17 ： 在终端检查 Java 环境是否是 17

```
java --verison
```

#### 4. 配置环境变量 MAC

克隆好适配鸿蒙的 Flutter 项目仓库后：

* 打开终端 & 打开环境变量配置文件 Mac 一般都是用户目录下 `.bash_profile` 

```shell
# 国内镜像
export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn

# 拉取下来的flutter_flutter/bin目录 
# 注意这里如果之前下载过 Google-Flutter 注释掉这个指定
export OH_FLUTTER_HOME=/Users/austenYang/Develop/ohos/flutter_ohos/flutter_flutter
export PATH=$PATH:$OH_FLUTTER_HOME/bin

# OpenHarmony SDK
export TOOL_HOME=/Applications/DevEco-Studio.app/Contents # mac环境
export DEVECO_SDK_HOME=$TOOL_HOME/sdk # command-line-tools/sdk
export PATH=$TOOL_HOME/tools/ohpm/bin:$PATH # command-line-tools/ohpm/bin
export PATH=$TOOL_HOME/tools/hvigor/bin:$PATH # command-line-tools/hvigor/bin
export PATH=$TOOL_HOME/tools/node/bin:$PATH # command-line-tools/tool/node/bin
export HDC_HOME=$TOOL_HOME/sdk/default/openharmony/toolchains # hdc指令（可选）
export PATH=$PATH:$HDC_HOME # hdc指令配置到环境变量 （可选）

# 可选配置项（防止由于Flutter OpenHarmony版的git下载地址环境变量不匹配，影响后续的flutter项目创建）
export FLUTTER_GIT_URL=https://gitcode.com/openharmony-sig/flutter_flutter.git

```

配置完成 执行`source .bash_profile`，使用 `flutter doctor -v` 检查环境是否配置正确。

#### 5. 创建Flutter项目& 构建 hap	

* 创建项目

```shell
# 创建 flutter 项目 方式一 ：只创建 ohos 平台
flutter create --platforms ohos <projectName>

# 创建 flutter 项目 方式二：创建 全平台 android、ios、ohos 、web 、桌面平台macos、windows
flutter create  <projectName>
```

* 构建项目，生成 hap 包

```shell
# 进入工程根目录
flutter build hap --debug
```

运行后生成鸿蒙使用的 hap 包 ，目录 `<ProjectName>/build/ohos/hap/entry-default-signed.hap`

#### 6. 运行 真机 & 模拟器

##### a. 真机运行

* 使用 DevEco-Studio 运作真机

打开 DevEco-Studio 后，open 项目目录 ：`<ProjectName>/ohos`。连接 鸿蒙Next真机 ，点击运行按钮。

**异常1：** 运行出现异常： Ohos-flutter 3.22.0 与 DevEco-Studo 5.1.1 Beta 连接真机 HarmonyOs Next:5.0.1.130 版本出现

```shell
compatibleSdkVersion and releaseType of the app do not match the apiVersion and releaseType on the device.
Troubleshooting guide
```

出现该问题是因为当前工程的兼容的最低版本（api 版本）高于设备镜像版本（api版本）。也就是说 ohos-flutter-3.22.0 版本创建的 flutter 项目兼容的最低版本大于HarmonyOs Next:5.0.1.130 版本。

问题解决：

使用命令行 `hdc shell param get const.ohos.apiversion` 当前鸿蒙真机或者是模拟器的 api 版本，对比看下工程级build-profile.json5配置的compatibleSdkVersion字段api版本。两种思路：

方式一：降低项目的 api 版本

方式二：升级设备的系统镜像版本

这里使用方式一，打开 flutter 对应的 ohos 项目，找到 `build-profile.json5` 修改 `compatibleSdkVersion": "5.1.0(18)"` 到 ``compatibleSdkVersion": "5.0.5(17)`` 。

[OHOS 所有版本相关信息网址](https://developer.huawei.com/consumer/cn/doc/harmonyos-releases/overview-allversion)

**异常2：** 运算鸿蒙项目需要签名

```
Install Failed: error: failed to install bundle.
code:9568320
error: no signature file.
Open signing configs
```

登录华为账号，进行 debug 签名即可。

* 使用 AndoridStudio 运行，使用 AndroidStudio 运行的前提是先使用 DevEco-Studio 签名：即上面异常2 处问题。

1. 使用 AndroidStudio 打开 : `<ProjectName>` 
2. 连接 鸿蒙手机 选择 mobile机器 运行即可，这里你连接的鸿蒙真机直接运行。同理 连接 Android 手机真机 也直接运行。

##### b. 模拟器运行

模拟器运行的策略和真机一样，使用 DevEco-Studio 和 Android Studio。

注意的就是 由于 鸿蒙真机和模拟器存在[差异](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/ide-emulator-specification-V5) ，会导致模拟器上，某些 Flutter 组件在鸿蒙上不能使用，现像就是直接崩溃。比如说 `FloatingActionButton` 包含阴影效果，就没法在模拟器中使用，会导致闪退。上图相关的 `API` 我们都要注意一下。

**可以的话，做 Flutter 鸿蒙化最好是使用真机。**



### 二、目前生态

纯 Dart （全部使用Dart代码，没有任何调用具体平台的代码）写的三方库一般都不需要适配鸿蒙

声网：

商业三方库：声网、一键登录、三方登录 官方不提供，这些调研后社区里面也没有，一般都需要自己适配。

| 三方Sdk                                              | 是否有 Fluter Sdk (Google)                                 | 是否支持鸿蒙平台         |
| ---------------------------------------------------- | ---------------------------------------------------------- | ------------------------ |
| 声网                                                 | 有 https://github.com/AgoraIO-Extensions/Agora-Flutter-SDK | 不支持                   |
| 云信                                                 | 有 https://github.com/netease-kit/nim-uikit-flutter        | 不支持                   |
| 一键登录                                             | 无                                                         | 无                       |
| QQ 微信 登录 https://github.com/RxReader/tencent_kit | 有                                                         | 有（功能不完善）         |
| 支付宝、微信 支付                                    | 有                                                         | 无                       |
| Svga (纯dart库)                                      | 有（ https://github.com/svga/SVGAPlayer-Flutter）需要适配  | 有<br />本地只有可以使用 |



### 



