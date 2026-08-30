---
title: "EU.org 免费二级域名申请教程"
date: 2026-08-30
draft: false
tags: ["免费域名", "域名申请"]
math: false
---

## 服务简介

EU.org 是 1996 年就开始运营的老牌非营利免费二级域名服务，提供 `*.eu.org` 后缀（如 `yourname.eu.org`）。它跟欧盟没有任何关系，纯靠志愿者维护。最大特点是**永久免费、无需续费**，且支持自定义 NS、DNSSEC、WHOIS 隐私保护，被 Google、Cloudflare 等广泛认可，适合个人博客、学习项目长期持有。

注意：它**不是自动开通**，每笔申请都由管理员人工审核，这是它最大的「坑」。

## 申请前置条件

- 一个能收验证邮件的真实邮箱（Gmail / Outlook 这类，别用临时邮箱）
- 能设置两台 NS 服务器（建议提前准备好 Cloudflare / DNSPod 的 NS）
- 耐心（审核可能数周，甚至数月）

## 逐步骤申请流程

1. 打开 https://nic.eu.org/ ，点击 **Signup / Register** 创建「Contact（联系人）」账号。
2. 填写用户名、密码、姓名、邮箱、地址等；建议把 **Private（不在公开 Whois 显示）** 勾上保护隐私；勾选接受域名政策后提交。
3. 查收激活邮件，点链接激活并登录。
4. 登录后进入域名申请页 https://nic.eu.org/arf ，点击 **New Domain**。
5. 输入想要的名称（如 `myblog.eu.org`），系统会校验是否可用。
6. 关键一步：填入**至少两台 NS 服务器**。我习惯先在 Cloudflare 添加域名拿到 NS 再回来填——Cloudflare 免费版给的两组 `*.ns.cloudflare.com` 就能直接用。
7. 提交后系统会校验 NS 是否有效。若报 DNS 错误，多半是 Cloudflare 还没生效，等一两分钟刷新即可。
8. 提交完成，进入**人工审核队列**，等邮件通知。

## 如何配置 DNS / 指向站点

EU.org 本身不托管解析，只认你填的 NS。我推荐两种方案：

- **Cloudflare**：添加 `myblog.eu.org` → 选免费计划 → 复制它给的 NS → 回填到 EU.org 申请表单。审核通过后，在 Cloudflare 里加 A / CNAME 记录把域名指向你的站点（GitHub Pages、Vercel、服务器 IP 等），还能顺手开启 HTTPS。
- **DNSPod**：同理，先在 DNSPod 添加域名拿到 NS，再回填 EU.org。

## 注意事项与常见坑

- **审核极慢**：官方说是志愿者审核，常见 1–7 天，但我也见过等了几个月甚至石沉大海的。别重复提交同一域名，会被直接拒绝。
- **NS 必须提前可用**：提交时 EU.org 会检查 NS，没先在外层解析商建好域名会一直报错。
- **永久有效、不回收**：只要不违反政策（如滥用、 spam），通过后基本不会被收回，也无需续费——这点比 us.kg 之类稳得多。
- **CY / GR / NL 等子域不在此受理**，会直接被拒。

## 参考链接

- EU.org 官网：https://www.eu.org/
- 申请后台：https://nic.eu.org/arf
- 图文教程（Cloudflare 方案）：https://indexedev.com/post/how-to-get-a-free-eu-org-domain-and-set-it-up-with-cloudflare
- DNSPod 托管攻略：https://www.mobufan.eu.org:666/archives/t6EPzjs7
