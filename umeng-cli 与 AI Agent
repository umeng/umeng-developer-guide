# 友盟 CLI 与 AI Agent 使用指南

> `umeng-cli` 是友盟官方开源命令行工具，可供人类用户和 AI Agent 调用友盟相关能力。官方仓库目前说明其覆盖 U-App、U-APM、U-Push、U-DOP、AppTrack 和 U-Mini 等服务，并区分 CLI 工具安装与 AI Agent Skills 安装。

## 一、适用场景

- 在终端中登录并调用友盟相关 API。
- 查询移动统计、性能监控、消息推送或数据开放相关能力。
- 为支持 Skills 的 AI Agent 提供友盟工具使用说明。
- 需要通过结构化命令减少手工进入多个控制台的操作。

## 二、安装 CLI

按照官方仓库当前 README，推荐使用 npm 全局安装：

```bash
npm install -g @umengfe/umeng-cli
```

安装后验证：

```bash
umeng-cli --version
```

## 三、登录与账号

开始登录：

```bash
umeng-cli login
```

如果终端不方便显示二维码：

```bash
umeng-cli login --no-qr
```

查看当前账号：

```bash
umeng-cli whoami
```

查看或切换已登录账号：

```bash
umeng-cli account list
umeng-cli account switch
```

## 四、为 AI Agent 安装 Skills

CLI 与 Skills 是两项不同安装。按照官方仓库当前说明，可执行：

```bash
npx skills add umeng/umeng-cli -y -g
```

CLI 负责实际命令和鉴权；Skills 帮助支持该机制的 AI Agent 理解可用能力、参数和操作流程。

## 五、安全使用原则

1. 不要把 Cookie、AccessKey、Secret 或登录配置提交到 Git。
2. AI Agent 调用前先确认账号和目标应用，尤其是推送或其他可能改变外部状态的操作。
3. 查询类操作尽量使用只读权限和最小时间范围。
4. 输出日志、截图或提交 Issue 前，对用户标识、Token 和凭证脱敏。
5. 不要复制官方 README 中标记为 TODO、尚未实现的安装方式执行。
6. 自动化环境应使用独立账号、最小权限和可审计的凭证管理方案。

## 六、卸载

卸载工具及配置：

```bash
umeng-cli uninstall
```

卸载工具但保留配置和凭证：

```bash
umeng-cli uninstall --keep-config
```

执行卸载前应先确认是否需要保留当前配置。

## 七、官方资料

- [umeng-cli 官方 GitHub 仓库](https://github.com/umeng/umeng-cli)
- [友盟开发者中心](https://developer.umeng.com/docs)
- [友盟 OpenAPI 入口](https://devs.umeng.com/api)

最后核对日期：2026-08-06  
维护团队：友盟开发者服务团队
