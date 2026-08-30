---
title: "is-a.dev 免费二级域名申请教程"
date: 2026-08-30
draft: false
tags: ["免费域名", "域名申请"]
math: false
---

## 服务简介

is-a.dev 是一个社区驱动的开源项目，专门给开发者发 `*.is-a.dev` 后缀（如 `yourname.is-a.dev`、`project.is-a.dev`）。它已经加入公共后缀列表（PSL），很多平台把它当正规顶级域对待，限制更少。本质是用 **GitHub PR 的方式**管理域名——没有传统注册表单，你的域名就是仓库里一个 JSON 文件。完全免费，由 Cloudflare 赞助支撑 DNS。

## 申请前置条件

- 一个 **GitHub 账号**（这是唯一的登录方式，不收邮箱/手机号）
- 一个打算指向的站点或项目（GitHub Pages、Vercel、Netlify、自有服务器 IP 都行）
- 确定要用的记录类型（最常用 CNAME 或 A）

## 逐步骤申请流程

1. 打开仓库 https://github.com/is-a-dev/register ，点右上角 **Fork** 把它复制到你的账号。
2. 进入你 fork 的 `domains/` 文件夹，点 **Add file → Create new file**。
3. 文件名就是你的子域名：`yourname.json` → 对应 `yourname.is-a.dev`。规则：2–64 个字符，只能小写字母、数字和连字符。
4. 写入 JSON 内容。GitHub Pages 示例：

```json
{
  "owner": { "username": "你的GitHub用户名" },
  "records": { "CNAME": "你的用户名.github.io" }
}
```

指向服务器 IP 则用 `"A": ["123.45.67.89"]`。还能加 MX / TXT / URL 重定向等。
5. 提交文件后，GitHub 会提示 **Open a Pull Request**，写清描述、勾选清单（确认是开发相关用途）后提交。
6. 维护者会审核，通常 **24–72 小时**合并。若被要求修改，记得按意见改完，否则会被拒。
7. 合并后 DNS 几分钟到 24 小时内生效，访问 `yourname.is-a.dev` 即可。GitHub Pages 用户记得在仓库设置里开启 **Enforce HTTPS**。

## 如何配置 DNS / 指向站点

DNS 记录全写在那份 JSON 里，合并即生效，不用去别的解析商。常见写法：

- CNAME：GitHub Pages / Vercel / Netlify 站点
- A / AAAA：自有服务器 IP
- URL：纯跳转（如跳转到你的主站）
- MX / TXT：邮件路由、域名验证

## 注意事项与常见坑

- **NS 记录受限**：默认不支持 NS 委托；真要用，需子域名存在 30 天以上并在 PR 里说明理由，否则 PR 会被关。绝大多数情况用 CNAME/A 就够了，别硬上 NS。
- **命名限制**：单字符、保留名（www/api/mail 等）注册不了，用户名别带大写和下划线。
- **维护靠社区**：不接受明显的商业滥用和政治用途（看 ToS）。改动或删除域名，直接编辑/删除你的 JSON 再提一个 PR 即可。
- 服务状态、停服公告主要发在官方 Discord，GitHub 只在严重事故时通知。

## 参考链接

- 注册仓库：https://github.com/is-a-dev/register
- 官方文档：https://docs.is-a.dev
- 步骤图文：https://dev.to/dthiwanka/get-your-free-is-adev-subdomain-step-by-step-guide-for-developers-27o7
