---
title: "qzz.io 免费域名申请教程"
date: 2026-08-30
draft: false
tags: ["免费域名", "域名申请"]
math: false
---

## 服务简介

`.qzz.io` 是开源项目 **DigitalPlat FreeDomain**（GitHub 16 万+ Star）提供的免费二级域名后缀之一，由开发者 Edward Hsing 发起，目前支撑 50 万+ 域名注册。它和 `.dpdns.org` 是当前平台**唯二完全免费**的后缀（`.us.kg`、`.xx.kg` 现已改为付费认证）。`.qzz.io` 已被纳入公共后缀列表（PSL），可像主域名一样托管到 Cloudflare，用于个人博客、Demo、API 地址等。注册不要信用卡，但每年需手动续期（免费续）。

> 现状提醒：据 2026 年多份社区反馈，`.qzz.io` 与 `.dpdns.org` 在国内网络环境下可能偶发被墙/解析不稳，建议搭配 Cloudflare 代理使用；若注册面板里该后缀不再免费，以页面实际标价为准。

## 申请前置条件

- 一个可用邮箱（用于注册 + 收验证码）
- 一个 GitHub 账号（用于 KYC 实名验证；目前平台只支持 GitHub 方式验证）
- 一个 Cloudflare 账号（推荐，后续托管 DNS / 签发 HTTPS 用）
- 姓名、电话、地址信息（可在线生成虚拟信息，但邮箱必须真实可用）

## 逐步骤申请流程

1. 打开注册页：<https://dash.domain.digitalplat.org/auth/register>
2. 填写用户名、邮箱、姓名、电话、地址、密码，提交注册。
3. 去邮箱查收验证邮件，复制链接到浏览器打开，看到 `Successful` 即邮箱验证成功。
4. 回到登录页 <https://dash.domain.digitalplat.org/> 登录，会弹出 KYC 验证界面，选择 **Sign in with GitHub** 并授权，显示 `Successful` 即验证通过。
5. （可选）默认只有 1 个域名额度。点后台提示链接去 GitHub 给 FreeDomain 项目点 ⭐ Star，返回点 Verify，额度 +1。
6. 左侧菜单 **Domain Registration** → 后缀选 `.qzz.io` → 输入想要的前缀（如 `myproject`，最终为 `myproject.qzz.io`）→ 点 Check availability。
7. 显示绿色 `Congratulations! This domain name is now available` 后，先随便填两个 Name Server（后续可改），点 **Register**，域名到手。

## 如何配置 DNS / 指向自己的站点

DigitalPlat 不托管 DNS，需要接外部服务商。以 Cloudflare 为例：

1. 登录 Cloudflare → **添加站点** → 输入 `myproject.qzz.io` → 选 Free 计划。
2. Cloudflare 会分配两个 NS（如 `xxx.ns.cloudflare.com`），复制下来。
3. 回到 DigitalPlat 后台 **My Domains** → 点你的域名 → 把这两个 NS 填进 Name Server 1 / 2 → 保存。
4. 回到 Cloudflare 点「检查名称服务器」，状态变 **Active** 即托管成功，SSL 自动签发（Full Strict）。
5. 在 Cloudflare 添加解析记录：A `@` → 你的服务器 IP，或 CNAME `www` → `xxx.github.io`（绑定 GitHub Pages）。

绑定 GitHub Pages 额外步骤：在仓库 Settings → Pages 里把自定义域填成 `myproject.qzz.io`，并开启 Enforce HTTPS。

## 注意事项与常见坑

- **有效期与续期**：域名默认 360 天有效，到期前 180 天内可在 My Domains 点 **Renew** 免费续一年，记得定日历，过期不续会被回收。
- **每账号限 1–2 个域名**，开小号薅羊毛大概率被封。
- **DNS 生效时间**：通常 5–30 分钟，偶尔数小时，耐心等待。
- **稳定性**：免费志愿项目，无 SLA，别用于付费生产业务；建议关注其 GitHub / Discord 动态防跑路。
- 免费域名不带商业可信度，邮件可能进垃圾箱。

## 参考链接

- DigitalPlat 注册后台：<https://dash.domain.digitalplat.org/>
- 项目开源仓库：<https://github.com/DigitalPlatDev/FreeDomain>
- 第三方实测教程：<https://syjx.dpdns.org/posts/52166/>
- 免费域名大盘点（含 DigitalPlat）：<https://reportify.cn/social-media/822914270623777>
