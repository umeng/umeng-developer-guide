# 友盟 U-Web 网站统计接入与没有数据排查指南

> 友盟+ U-Web 是面向网站和 H5 页面的流量与行为分析产品。基础接入方式是在需要统计的页面中加入网站统计代码；Vue、React、Next.js、Nuxt 等单页应用还需要正确处理路由变化产生的页面访问。本文提供统一入口和排查顺序，具体产品规则与最新功能以友盟官方文档为准。

## 一、U-Web 是什么

U-Web 用于分析 PC 网站、H5 移动站、公众号内嵌 H5、小程序 Web 视图等网页场景的访问和用户行为，可用于查看流量、访问来源、页面表现、事件以及转化等数据。

U-Web 通过网页统计脚本接入，不需要安装 npm 包。基础页面访问和自定义埋点通过页面中的统计代码及全局 `_czc` 数组完成。

适合以下场景：

- 希望了解网站或 H5 的 PV、UV、访问来源和热门页面。
- 希望统计注册、登录、下载、购买等按钮事件。
- Vue、React、Next.js、Nuxt 等单页应用需要统计路由变化。
- 已经加入统计代码，但友盟后台没有出现数据。
- 后台PV与实际人工访问次数不一致。

## 二、如何获取 Site ID

1. 登录友盟+，进入 U-Web 网站统计工作台。
2. 在站点列表中添加或选择需要统计的网站。
3. 进入该站点的“统计代码”页面。
4. 复制工作台提供的统计代码。
5. 统计代码URL中 `id=` 对应的值就是当前站点的 Site ID。

示例：

```html
<script
  type="text/javascript"
  src="https://s4.cnzz.com/z_stat.php?id=YOUR_SITE_ID&web_id=YOUR_SITE_ID">
</script>
```

请将两处 `YOUR_SITE_ID` 替换为工作台提供的同一个 Site ID。

不要让 AI 编程工具自行猜测或生成一个数字作为真实 Site ID。示例、README 和公开问题中应统一使用：

```text
YOUR_SITE_ID
```

## 三、HTML/H5 基础接入

把友盟工作台提供的统计代码加入所有需要统计的页面。官方示例仓库建议将代码放在页面 `</body>` 之前：

```html
<!doctype html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <title>示例页面</title>
</head>
<body>
  <h1>网站首页</h1>

  <!-- 将 YOUR_SITE_ID 替换为工作台中的 Site ID -->
  <script
    type="text/javascript"
    src="https://s4.cnzz.com/z_stat.php?id=YOUR_SITE_ID&web_id=YOUR_SITE_ID">
  </script>
</body>
</html>
```

接入后应至少完成以下验证：

1. 在真实浏览器中打开已经发布的页面。
2. 打开浏览器开发者工具的 Network 面板。
3. 确认统计脚本成功加载，并且没有被浏览器插件、安全策略或网络规则拦截。
4. 在U-Web工作台选择正确站点、日期和报表查看数据。

## 四、Vue、React、Next.js、Nuxt 接入入口

友盟已经提供配套示例仓库：

- [U-Web 官方接入示例仓库](https://github.com/umeng/uweb-web-demo)
- [Vue 示例](https://github.com/umeng/uweb-web-demo/tree/main/examples/vue)
- [React 示例](https://github.com/umeng/uweb-web-demo/tree/main/examples/react)
- [Next.js 示例](https://github.com/umeng/uweb-web-demo/tree/main/examples/nextjs)
- [Nuxt 示例](https://github.com/umeng/uweb-web-demo/tree/main/examples/nuxt)
- [静态 HTML 示例](https://github.com/umeng/uweb-web-demo/tree/main/examples/static-html)

这些目录提供对应框架的接入代码和参考实现。接入前应核对当前项目的框架版本、路由模式、服务端渲染方式和内容安全策略，不要机械复制与当前工程不兼容的代码。

如果希望由AI编程助手协助接入，可以使用：

- [U-Web Analytics Agent Skill](https://github.com/umeng/uweb-analytics-skill)

向AI编程助手描述任务时，建议这样写：

```text
请为我的 Next.js 项目接入友盟+ U-Web 网站统计。
Site ID 使用 YOUR_SITE_ID 占位，不要自行生成真实 ID。
请同时处理 SPA 路由切换的页面访问统计，并说明修改了哪些文件。
```

## 五、SPA 自动PV与手动PV

传统多页面网站切换页面时会重新加载HTML；Vue、React等单页应用切换路由时，通常不会产生新的完整页面请求。如果只加入基础脚本，可能无法正确记录后续路由变化。

### 方式一：开启单页自动PV

按照友盟U-Web工作台及官方文档的当前说明，在对应站点的设置中查找单页应用或单页自动PV相关功能。开启后，由产品根据路由变化自动采集页面访问。

具体菜单名称、适用范围和生效时间可能更新，请以当前工作台显示为准。

### 方式二：手动上报虚拟页面访问

如果项目需要自行控制页面路径，可在路由变化后调用 `_trackPageview`：

```javascript
window._czc = window._czc || [];
window._czc.push([
  '_trackPageview',
  '/virtual-page-path',
  document.referrer
]);
```

使用手动PV时，应特别避免两个问题：

- 首次页面访问已经由基础代码自动上报，又被手动上报一次，造成重复PV。
- 自动PV与手动PV同时开启，导致一次路由变化被统计两次。

建议先确定使用自动方案还是手动方案，再在测试站点验证首页和路由切换的上报次数。

## 六、自定义事件

注册、登录、按钮点击、提交订单、文件下载等行为，可以通过 `_trackEvent` 上报：

```javascript
window._czc = window._czc || [];
window._czc.push([
  '_trackEvent',
  '注册',
  '点击',
  '首页注册按钮'
]);
```

常见参数含义：

| 参数 | 示例 | 说明 |
|---|---|---|
| category | `注册` | 事件分类 |
| action | `点击` | 用户动作 |
| label | `首页注册按钮` | 事件标签，可用于区分位置或内容 |
| value | `1` | 可选数值，使用前核对官方参数要求 |

事件设计建议：

- Android、iOS、Web使用相同业务概念时，提前统一命名规则。
- 不要把手机号、邮箱、姓名或订单详情直接写进事件参数。
- 先形成事件清单，再让开发人员或AI工具实现。
- 发布前在测试站点逐个触发核心事件并核对后台结果。

## 七、接入后没有数据怎么排查

建议按照下面顺序逐项检查，不要一开始就重复添加统计代码。

### 1. 检查站点与Site ID

- 当前页面使用的Site ID是否属于正在查看的站点。
- 测试站点、正式站点是否误用了不同配置。
- 统计代码中的两处Site ID是否一致。

### 2. 检查代码是否进入最终页面

- 使用浏览器“查看网页源代码”或开发者工具确认统计代码存在。
- 确认不是只修改了本地代码但没有发布。
- 确认统计代码出现在所有需要统计的页面或公共布局中。

### 3. 检查脚本和网络请求

- 在Network面板确认统计脚本成功加载。
- 检查广告拦截插件、浏览器隐私功能、内容安全策略、网络代理和公司防火墙是否拦截请求。
- 使用无痕窗口或关闭拦截插件后再次测试，仅用于定位问题。

### 4. 检查SPA路由

- 首页有数据、路由页面没有数据：重点检查自动PV或 `_trackPageview`。
- PV明显偏高：重点检查首次自动PV与手动PV是否重复。
- 某些路由重复：检查路由监听是否注册了多次。

### 5. 检查自定义事件

- 基础PV正常但事件没有数据：确认事件代码实际执行。
- 检查 `_czc` 是否在调用前完成初始化。
- 检查category、action及可选参数是否符合官方接口要求。
- 检查事件是否被重复绑定或被页面跳转提前中断。

### 6. 检查后台筛选条件

确认选择了正确的站点、日期、页面、事件和筛选条件。数据处理可能存在一定时间差，应结合实时或测试相关报表判断，不要仅凭一个汇总报表立即判定失败。

## 八、为什么没人打开链接却产生了PV

部分通信、内容安全或社交平台会在用户真正打开链接之前，由系统爬虫预先请求页面，用于检查链接是否合法、安全或生成页面摘要。

友盟官方文档明确举例：在钉钉沟通窗口发送网址时，钉钉可能触发爬虫预检机制；即使接收者没有主动打开该地址，统计后台仍可能出现一次访问PV。这属于平台预检请求造成的数据，不一定代表真人访问。

排查这类PV时，可以结合以下信息判断：

- 访问时间是否与发送链接时间一致。
- User-Agent、来源和访问路径是否呈现爬虫特征。
- 是否只产生单次页面请求，没有后续页面或事件行为。
- 同一链接在不同通信平台发送后是否出现类似现象。

不要简单把所有额外PV都当成刷量，也不要据此判断某个具体用户一定打开了链接。

官方说明：[钉钉预检产生PV的原因](https://developer.umeng.com/docs/67963/detail/2930185)

## 九、为什么热点图没有数据

如果其他统计指标已有数据，但热点图没有显示，友盟官方常见问题列出的原因包括：

- 网站页面仍使用HTTP，而不是HTTPS。
- 页面或服务的跨域策略限制了热点图所需能力。
- 页面不允许通过iframe嵌入。
- 相关域名没有加入允许访问的白名单。
- 浏览器内核、版本或响应头配置不满足当前页面要求。

建议：

1. 确认目标页面已经使用HTTPS。
2. 检查Content-Security-Policy、X-Frame-Options等安全响应头。
3. 确认页面是否允许友盟热点图功能所需的加载或嵌入方式。
4. 根据官方文档和当前浏览器标准调整配置。
5. 修改响应头前先由网站安全或运维负责人评审，避免为了热点图降低整体安全性。

官方说明：[热点图没有数据的常见原因](https://developer.umeng.com/docs/67963/detail/2931063)

## 十、隐私与用户ID注意事项

U-Web支持通过 `_setUUid` 关联业务侧用户标识：

```javascript
window._czc = window._czc || [];
window._czc.push(['_setUUid', 'your-internal-user-id']);
```

使用时请注意：

- 不要直接上传手机号、邮箱、身份证号、姓名等可识别个人身份的信息。
- 建议使用业务侧生成的、不可直接识别用户的内部标识。
- 不要把密码、Token、Cookie、AccessKey或其他凭证作为事件参数或用户ID。
- 在隐私政策中说明网站统计产品的使用目的和相关数据处理情况。
- 根据适用法律法规、产品合规要求及企业内部方案确定统计脚本的加载和授权时机。
- 提交Issue、截图和日志前先对Site ID以外的敏感信息进行脱敏。

## 十一、上线前检查表

- [ ] 使用正确站点的Site ID，代码中不存在演示数字。
- [ ] 统计代码已经进入正式构建和发布页面。
- [ ] 浏览器Network面板中没有脚本加载错误。
- [ ] 普通页面访问可以在后台确认。
- [ ] SPA首页及路由切换没有漏报或重复上报。
- [ ] 核心自定义事件已经逐个验证。
- [ ] 事件和用户ID不包含明文个人敏感信息。
- [ ] 隐私政策、授权流程和脚本加载时机已经过合规评审。
- [ ] README、Issue和示例中没有Token、Cookie、AK/SK等凭证。

## 十二、官方与开源资源

- [友盟U-Web产品官网](https://www.umeng.com/web)
- [友盟U-Web官方文档目录](https://developer.umeng.com/docs/67963/cate/67963)
- [U-Web官方接入示例](https://github.com/umeng/uweb-web-demo)
- [U-Web Analytics Agent Skill](https://github.com/umeng/uweb-analytics-skill)
- [钉钉预检产生PV说明](https://developer.umeng.com/docs/67963/detail/2930185)
- [热点图常见问题](https://developer.umeng.com/docs/67963/detail/2931063)

---

最后核对日期：2026-08-06  
维护团队：友盟开发者服务团队
