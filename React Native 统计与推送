# 友盟 React Native 统计与推送集成指南

> 友盟官方 React Native 示例仓库提供 Android/iOS 桥接示例，覆盖统计、推送和分享等能力。仓库 README 明确说明：下载内容主要是桥接文件，并不包含最新原生 SDK，实际使用时仍需从官方下载中心获取对应组件并核对兼容性。

## 一、开始前先确认

1. 明确需要的产品：U-App、U-Push、U-Share，或其组合。
2. 明确目标平台：Android、iOS，或两端同时支持。
3. 记录 React Native、Android Gradle、Xcode 和系统最低版本。
4. 查看官方示例仓库当前分支与 README 的版本说明。
5. 从友盟 SDK 下载中心获取当前原生组件，不要把示例仓库中的旧依赖直接视为最新正式 SDK。

## 二、Android 集成思路

1. 按示例仓库说明复制公共桥接文件和所需产品的桥接模块。
2. 如果包路径与示例不同，修改桥接文件中的包路径。
3. 在 React Native 的 `Application` 中注册对应 Package。
4. 按当前原生 Android 文档集成友盟公共组件及所需产品 SDK。
5. 在隐私授权流程完成后执行正式初始化。
6. 使用统计时验证页面和事件；使用推送时验证注册回调、Device Token 和单设备推送。

## 三、iOS 集成思路

1. 将当前友盟原生 SDK 加入 iOS 工程。
2. 添加所需组件的桥接文件和初始化配置。
3. 在 AppDelegate 中完成对应产品初始化。
4. 根据原生 iOS 文档配置系统权限、推送证书及回调。
5. 从 JavaScript 层调用桥接接口，并分别验证统计事件和推送回调。

## 四、常见问题

### 1. 原生能编译，JavaScript 调用不到

检查桥接模块是否注册、导出名称是否一致、包路径是否正确，以及修改原生代码后是否重新构建了应用。

### 2. Android 出现依赖冲突

核对各友盟产品对公共基础组件的版本要求，避免不同模块重复引入不兼容版本。以当前原生文档和下载中心为准。

### 3. 推送收不到

先按原生平台验证 SDK 注册和单设备推送，再排查 JavaScript 桥接。React Native 层无法替代 APNs、Android通知权限或厂商通道的原生配置。

### 4. 示例代码与当前工程差异很大

官方示例仓库包含历史兼容说明。不要机械复制；先核对当前 React Native 和原生构建环境，再以官方原生 SDK 文档为基础适配桥接层。

## 五、官方资料

- [友盟 React Native 官方示例仓库](https://github.com/umeng/React_Native_Compent)
- [友盟 SDK 下载中心](https://devs.umeng.com/sdk)
- [U-App Android 集成](https://developer.umeng.com/docs/119267/cate/118577)
- [U-App iOS 集成](https://developer.umeng.com/docs/119267/cate/119475)
- [U-Push Android 集成](https://developer.umeng.com/docs/67966/detail/206987)
- [U-Push iOS 集成](https://developer.umeng.com/docs/67966/detail/66734)

最后核对日期：2026-08-06  
维护团队：友盟开发者服务团队
