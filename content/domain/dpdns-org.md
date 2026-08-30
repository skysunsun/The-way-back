---
title: "dpdns.org 免费域名申请教程"
date: 2026-08-30
draft: false
tags: ["免费域名", "域名申请"]
math: false
---

## 服务简介

`.dpdns.org` 同样是 **DigitalPlat FreeDomain** 提供的免费二级域名后缀，与 `.qzz.io` 并列当前平台唯二免费项。它已被纳入公共后缀列表（PSL），拥有主域名级别的完整功能，可托管到 Cloudflare，也常被用于绑定 GitHub Pages、做内网 DDNS。完全免费、无需信用卡，但需每年手动续期（免费续）。据社区反馈，`.dpdns.org` 在国内网络下偶发被墙，建议走 Cloudflare 代理。

> 现状提醒：早期免费开放的 `.us.kg` 现已改为收费（约 2 美元认证），新用户只白嫖 `.dpdns.org` / `.qzz.io`；注册时以面板实际标价为准。

## 申请前置条件

- 一个真实可用邮箱（收验证码）
- 一个 GitHub 账号（KYC 实名验证目前仅支持 GitHub OAuth）
- 推荐准备 Cloudflare 账号（托管 DNS + 免费 HTTPS）
- 用户名、姓名、电话、地址（可用虚拟信息生成器，但邮箱须真实）

## 逐步骤申请流程

1. 打开注册页：<https://dash.domain.digitalplat.org/auth/register>
2. 填写用户名、邮箱、姓名、电话、地址、密码并提交。
3. 去邮箱点开验证链接，出现 `Successful` 即邮箱验证成功。
4. 登录 <https://dash.domain.digitalplat.org/>，在 KYC 界面选 **Sign in with GitHub** 授权，显示 `Successful` 即验证通过。
5. （可选）默认 1 个额度。点后台链接去 GitHub 给项目点 ⭐ Star，返回 Verify 可 +1 额度。
6. 左侧 **Domain Registration** → 后缀选 `.dpdns.org` → 输入前缀（如 `myblog`，即 `myblog.dpdns.org`）→ Check availability。
7. 显示绿色可用提示后，随便填两个 Name Server（之后能改），点 **Register** 即注册成功。

## 如何配置 DNS / 指向自己的站点

以 Cloudflare + GitHub Pages 为例：

1. 登录 Cloudflare → **添加站点** → 输入 `myblog.dpdns.org` → 选 Free 计划。
2. 复制 Cloudflare 分配的两个 NS（如 `aaa.ns.cloudflare.com` / `bbb.ns.cloudflare.com`）。
3. 回 DigitalPlat 后台 **My Domains** → 点域名 → 把 NS 填进 Name Server 1 / 2 → 保存（Register）。
4. Cloudflare 侧点「检查名称服务器」，变 **Active** 即托管完成，SSL 自动签发。
5. 在 Cloudflare 添加记录：
   - 绑定 GitHub Pages：CNAME `@` → `yourname.github.io`（同时仓库 Settings → Pages 填自定义域 `myblog.dpdns.org`，开启 Enforce HTTPS）；
   - 普通服务器：A `@` → 服务器 IPv4。
6. 非 HTTP 服务（SSH、游戏服）记得把橙色云朵点灰，关闭代理。

## 注意事项与常见坑

- **续期**：默认 360 天有效期，到期前 180 天内点 **Renew** 免费续一年，设提醒避免被回收。
- **额度限制**：每账号 1–2 个，别批量小号薅。
- **NS 生效**：通常几分钟到半小时，个别情况数小时；改 NS 后耐心等。
- **稳定性**：免费志愿项目，无 SLA，仅供学习/演示/个人项目，勿用于生产业务。
- 已通过 PSL，所以子域（如 `api.myblog.dpdns.org`）可被外部服务正确识别。

## 参考链接

- DigitalPlat 注册后台：<https://dash.domain.digitalplat.org/>
- 保姆级图文教程（CSDN）：<https://devpress.csdn.net/v1/article/detail/160453720>
- 社区实测（含 Cloudflare 托管）：<https://syjx.dpdns.org/posts/52166/>
- 建站指南-域名篇：<https://coookie.dpdns.org/posts/Building-Site/>
