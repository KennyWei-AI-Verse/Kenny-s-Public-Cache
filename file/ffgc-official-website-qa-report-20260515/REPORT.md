# FFGC 官网 QA 测试报告

测试站点：https://ffgcoffical.run.ingarena.net/

测试时间：2026-05-15 GMT+8

## 1. 测试范围

已覆盖：
- 未登录访问首页 / 登录页
- 中英文切换
- Google 登录跳转
- 常见业务路由未登录拦截
- 静态资源加载
- 桌面端 Chrome 1440×900
- 移动端 390×844
- 基础 SEO / 分享配置
- 基础安全响应头

未覆盖：
- 登录后的赛事中心页面
- 队伍 / 赛程 / 排名 / 报名 / 招募等登录后功能
- 竞猜活动功能

原因：当前站点所有核心业务路由都会重定向到 `/login`，未提供测试账号 / 已登录态。

## 2. 总体结论

当前公开入口可正常访问，登录页无白屏、无控制台错误，Google OAuth 跳转正常，静态资源加载正常。

但本轮无法完成真正“全量业务功能测试”，因为核心功能均在登录后。建议补充一个测试账号或临时测试登录态后，再做第二轮完整 QA。

## 3. 问题统计

- P0：0
- P1：1
- P2：3
- P3：5

## 4. Top Issues

### P1｜缺少测试账号 / 登录态，导致核心功能无法测试

【页面 / 模块】
全站核心业务模块

【实际结果】
访问 `/home`、`/schedule`、`/rules`、`/teams`、`/register`、`/recruit`、`/live`、`/ranking`、`/admin` 均 302 到 `/login`。

【期望结果】
如果需要大家帮忙 QA，应提供测试账号、白名单登录方式或测试环境免登录入口。

【影响】
无法确认登录后功能是否存在 Bug，也无法测试竞猜入口是否隐藏 / 禁用。

---

### P2｜移动端登录页首屏空间利用偏低

【页面 / 模块】
移动端登录页

【实际结果】
390×844 下，顶部区域较高，Hero 标题与 CTA 之间留白较大，首屏信息密度偏低。

【期望结果】
移动端首屏应更快看到赛事标题 + 登录按钮，减少无信息留白。

【建议】
移动端可压缩 topbar 高度，略微上移主视觉标题和登录按钮。

截图：`screenshots/mobile-login-zh.png`

---

### P2｜分享 / SEO 基础信息缺失

【页面 / 模块】
登录页 HTML Head

【实际结果】
检测到：
- 有 `<title>`
- 有 viewport
- 未发现 meta description
- 未发现 Open Graph 分享标签

【期望结果】
赛事官网通常需要配置分享标题、描述、预览图，便于在 IM / 社群中传播。

【建议】
补充：
- `meta name="description"`
- `og:title`
- `og:description`
- `og:image`
- `twitter:card`

---

### P2｜当前只能看到登录页，竞猜入口状态无法验证

【页面 / 模块】
竞猜活动

【实际结果】
未登录状态下未看到竞猜入口；登录后页面无法访问。

【期望结果】
如果竞猜未完成，登录后也应明确处理：隐藏 / 禁用 / Coming Soon。

【建议】
登录后补测时重点确认：竞猜入口不要进入空白页或报错页。

---

### P3｜中文按钮中 “GOOGLE” 被全大写，中文语境略硬

【页面 / 模块】
中文登录页 CTA

【实际结果】
按钮显示为「使用 GOOGLE 登录」。

【期望结果】
中文页面更自然的写法是「使用 Google 登录」。

【建议】
避免对登录按钮全局 `text-transform: uppercase`，或对中文场景单独覆盖。

---

### P3｜登录页可见说明较少，新用户理解成本略高

【页面 / 模块】
登录页 Hero

【实际结果】
说明文案「请先使用 Google 登录，进入 FFGC 官方赛事中心。」存在于 sr-only，不可见。

【期望结果】
视觉层面可以保留一行短说明，降低新用户疑惑。

【建议】
在 CTA 附近加一行小字，例如：「登录后查看赛程、队伍、报名与赛事信息」。

---

### P3｜安全响应头仍可补充 CSP / Referrer-Policy

【页面 / 模块】
HTTP 响应头

【已存在】
- `x-frame-options: DENY`
- `x-content-type-options: nosniff`
- `strict-transport-security`

【建议补充】
- `Content-Security-Policy`
- `Referrer-Policy`
- `Permissions-Policy`

---

### P3｜站点域名拼写为 offical，容易被误认为 typo

【页面 / 模块】
测试站点 URL

【实际结果】
域名为 `ffgcoffical.run.ingarena.net`，其中 `offical` 不是常见拼写 `official`。

【建议】
如果是临时测试域名可忽略；如果会长期对外传播，建议确认是否需要更正或增加跳转别名。

---

### P3｜英文页语言切换仍显示「简中」

【页面 / 模块】
英文登录页语言切换

【实际结果】
英文页上语言切换显示「简中 / EN」。

【建议】
可以接受；如追求更国际化，可显示为「中文 / EN」或「简体中文 / EN」。

## 5. 路由检查结果

| 路由 | 结果 |
|---|---|
| `/` | 302 → `/login` |
| `/login` | 200 |
| `/lang/zh` | 302 → `/` |
| `/lang/en` | 302 → `/` |
| `/home` | 302 → `/login` |
| `/schedule` | 302 → `/login` |
| `/rules` | 302 → `/login` |
| `/teams` | 302 → `/login` |
| `/register` | 302 → `/login` |
| `/recruit` | 302 → `/login` |
| `/live` | 302 → `/login` |
| `/ranking` | 302 → `/login` |
| `/admin` | 302 → `/login` |
| `/auth/google` | 302 → Google OAuth |

## 6. 静态资源检查

以下资源均返回 200：

- `/assets/favicon.svg`
- `/assets/ffgc-brand-logo.png`
- `/assets/ffgc-global-cup-title.png`
- `/assets/ffgc-banner.png`
- `/assets/feature-recruit-bg.webp`
- `/assets/feature-rules-bg.webp`
- `/assets/feature-schedule-bg.webp`
- `/assets/register-hero-bg.webp`
- `/assets/rules-hero-bg.webp`
- `/assets/schedule-hero-bg.webp`
- `/styles.css`

## 7. 建议下一轮补测清单

拿到测试账号 / 登录态后，优先补测：

1. 首页 / 赛事中心完整信息展示
2. 赛程、规则、队伍、报名、招募、直播、排名页面
3. 竞猜入口隐藏 / 禁用 / Coming Soon 状态
4. 移动端登录后全链路
5. 表单提交与错误提示
6. 权限边界：普通用户 vs 管理员
7. 外链和社媒入口
8. 长队名、空数据、未开始 / 进行中 / 已结束状态

## 8. 截图证据

- 桌面端登录页：`screenshots/desktop-login-zh.png`
- 移动端登录页：`screenshots/mobile-login-zh.png`
