---
title: "DigitalPlat 免费域名（.qzz.io / .dpdns.org）申请教程"
date: 2026-08-30
draft: false
tags: ["免费域名", "部署"]
math: false
---

## 服务简介

`.qzz.io` 与 `.dpdns.org` 都来自开源项目 **DigitalPlat FreeDomain**（GitHub 16 万+ Star），是平台**唯二完全免费**的二级域名后缀（`.us.kg`、`.xx.kg` 现已改为付费认证）。两者都已纳入公共后缀列表（PSL），可像主域名一样托管到 Cloudflare，用于个人博客、Demo、API 地址等。免费、无需信用卡，但每年需手动续期（免费续）。

> 现状提醒：据 2026 年社区反馈，这两个后缀在国内网络下可能偶发被墙/解析不稳，建议走 Cloudflare 代理；注册面板若显示收费，以页面实际标价为准。

## 申请前置条件

- 一个真实可用邮箱（收验证码）
- 一个 GitHub 账号（KYC 实名验证目前仅支持 GitHub OAuth）
- 推荐准备 Cloudflare 账号（托管 DNS + 免费 HTTPS）

## 逐步骤申请流程（带邀请链接）

通过邀请链接注册可直接进入注册页：<https://dash.domain.digitalplat.org/signup?ref=vUKq088Qwp>

1. 填写用户名、邮箱、姓名、电话、地址、密码并提交。

   ![DigitalPlat 邀请注册页（填写用户名、邮箱、密码等）](/img/tech/digitalplat-1.png)

2. 去邮箱点开验证链接，出现 `Successful` 即邮箱验证成功。
3. 登录 <https://dash.domain.digitalplat.org/>，在 KYC 界面选 **Sign in with GitHub** 授权，显示 `Successful` 即验证通过。

   ![GitHub KYC 授权成功提示](/img/tech/digitalplat-2.png)

4. （可选）默认 1 个额度。点后台链接去 GitHub 给 FreeDomain 项目点 ⭐ Star，返回 Verify 可 +1 额度。
5. 左侧 **Domain Registration** → 后缀选 `.qzz.io` 或 `.dpdns.org` → 输入前缀（如 `myblog`，即 `myblog.qzz.io` / `myblog.dpdns.org`）→ Check availability。

   ![Domain Registration 选择后缀并检查可用性](/img/tech/digitalplat-3.png)

6. 显示绿色可用提示后，随便填两个 Name Server（之后能改），点 **Register** 即注册成功。

   ![域名可用提示](/img/tech/digitalplat-4.png)

   ![注册成功后的 My Domains 列表](/img/tech/digitalplat-5.png)

## 如何配置 DNS / 指向自己的站点

以 Cloudflare + GitHub Pages 为例：

1. 登录 Cloudflare → **添加站点** → 输入完整域名 → 选 Free 计划。
2. 复制 Cloudflare 分配的两个 NS（如 `aaa.ns.cloudflare.com` / `bbb.ns.cloudflare.com`）。
3. 回 DigitalPlat 后台 **My Domains** → 点域名 → 把 NS 填进 Name Server 1 / 2 → 保存。

   ![在 DigitalPlat 域名管理中填写 Cloudflare NS](/img/tech/digitalplat-6.png)

4. Cloudflare 侧点「检查名称服务器」，变 **Active** 即托管完成，SSL 自动签发。
5. 添加记录：CNAME `@` → `yourname.github.io`（同时仓库 Settings → Pages 填自定义域，开启 Enforce HTTPS）；普通服务器用 A `@` → 服务器 IP。非 HTTP 服务（SSH、游戏服）记得把橙色云朵点灰。

## 注意事项与常见坑

- **续期**：默认 360 天有效期，到期前 180 天内点 **Renew** 免费续一年，设提醒避免被回收。
- **额度限制**：每账号 1–2 个，别批量小号薅。
- **NS 生效**：通常几分钟到半小时，个别情况数小时。
- **稳定性**：免费志愿项目，无 SLA，仅供学习/演示/个人项目，勿用于生产业务。

## 参考链接

- 邀请注册（含 +1 额度）：<https://dash.domain.digitalplat.org/signup?ref=vUKq088Qwp>
- 项目开源仓库：<https://github.com/DigitalPlatDev/FreeDomain>
- 社区实测（含 Cloudflare 托管）：<https://syjx.dpdns.org/posts/52166/>
