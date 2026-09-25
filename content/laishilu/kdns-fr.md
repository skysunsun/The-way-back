---
title: "kdns.fr 免费域名申请教程"
date: 2026-08-30
draft: false
tags: ["免费域名", "部署"]
math: false
---

## 服务简介

`.kdns.fr` 是法国免费云托管平台 **KataBump**（主打 Discord 机器人托管）提供的二级域名后缀，号称**永久免费、无需续费、无需信用卡**。它已被纳入公共后缀列表（PSL），被 DNS / 浏览器 / 各类托管系统当作真实域名看待。可申请 `yourname.kdns.fr` 这种真正的二级域名，违规内容（违法、钓鱼、spam）会被回收。

> 现状提醒：截至 2026 年 8 月，该服务仍正常开放，每账号最多 **2 个** `.kdns.fr` 域名，官方暂未提供加量方式；无有效期，不违规即可长期用。

## 申请前置条件

- 一个常用邮箱（收验证码；也支持 Discord 一键登录）
- 密码（8 位以上，含大小写）
- 推荐准备 Cloudflare 账号（托管 DNS + 免费 HTTPS + CDN）

## 逐步骤申请流程（带邀请链接）

通过邀请链接进入注册后台：<https://rl.katabump.fr/4cd747>

1. 点 **Create an account**，填写用户名、邮箱、密码（可走 Discord 一键登录），勾选协议创建账户。

![KataBump 注册/登录入口页（含「Create an account」与「Login with Discord」）](/img/tech/kdns-1.png)
2. 去邮箱收验证码（留意垃圾箱），填入完成邮箱验证，再做一次人机校验，进入后台。
3. 左侧菜单 **My Domains**（有的界面写作「mDomains」）→ 点 **Order a Domain** / 「Order my first domain」。

![Order a domain 表单：输入前缀、解析方式选 External NS、完成人机验证后点 Order the domain](/img/tech/kdns-2.png)
4. 输入至少 **3 位**的前缀（如 `myname`，最终为 `myname.kdns.fr`），解析方式选外部 NS（External NS，之后可改）。
5. 点验证 → 勾选阅读协议 → **Order the domain**。若提示 `Domain already exists` 说明被占用，换前缀重试。
6. 申请成功后，后台会提示配置 Nameservers，进入下一步托管。

![KataBump 后台「Domain created! Now configure your nameservers」成功提示 + Nameservers 配置面板（NS1 / NS2 与 Save 保存按钮）](/img/tech/kdns-3.png)

## 如何配置 DNS / 指向自己的站点

以 Cloudflare 托管为例：

1. 登录 Cloudflare → **添加站点** → 输入完整域名 `myname.kdns.fr` → 选 Free 计划。
2. 复制 Cloudflare 分配的两个 NS（如 `anna.ns.cloudflare.com` / `ben.ns.cloudflare.com`）。

![Cloudflare 「更新您的名称服务器来激活 Cloudflare」页面，列出为该域名分配的两个 NS（图中红圈标出）](/img/tech/kdns-4.png)
3. 回 KataBump 后台找到 **Nameservers**，把这两个 NS 填进 NS1 / NS2，点 **Save Nameservers**。
4. 返回 Cloudflare 点「我已更新名称服务器，立即检查」，通常几分钟即变 **Active**，SSL 自动签发。
5. 添加解析：A `@` → 服务器 IPv4（橙色云朵开启启用 CDN）；CNAME `blog` → 目标域名（如 GitHub Pages / Vercel）。SSH / 游戏服等非 HTTP 服务需把云朵点灰关闭代理。

## 注意事项与常见坑

- **永久免费 ≠ 无风险**：不违反条款（违法、钓鱼、冒充、滥用）可一直用，被举报到 signalement.kdns.fr 会回收。
- **每账号上限 2 个**，无加量入口，别指望无限开。
- **前缀至少 3 位**，太短或常见词易占用，备几个备选。
- **稳定性**：依赖 KataBump 运营，免费无 SLA，重要业务建议备一个付费域名兜底。

## 参考链接

- 邀请注册链接：<https://rl.katabump.fr/4cd747>
- KataBump 免费域名官方页：<https://katabump.com/en/free-domain>
- 注册后台：<https://dashboard.katabump.com>
