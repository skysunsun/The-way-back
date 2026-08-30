---
title: "Stackryze Domains 免费子域名申请教程"
date: 2026-08-30
draft: false
tags: ["免费域名", "免费主机"]
math: false
---

# Stackryze Domains 免费子域名申请教程

前阵子想给 Argo 隧道找个"伪装域名"，搜到一家叫 Stackryze 的印度小团队，号称永久免费、可接 Cloudflare，实测确实能白嫖。下面是我 2026-08 核实后的真实流程。

## 服务简介
Stackryze 是一个开源、社区驱动的免费子域名服务，由一群学生开发者运营。提供完整 DNS 控制权，可指向 GitHub Pages、Vercel、Cloudflare Pages 或自己的 VPS。**完全免费、无需信用卡、无需身份证、无审核等待**。可用后缀：`*.indevs.in`、`*.sryze.cc`、`*.ryzedns.org`、`*.nx.kg`，近期还新增了 `*.ryzn.pro`。官方强调 "Forever Free"，但有 1 年有效期需手动续期。

## 申请前置条件
- 一个 GitHub 账号（官方主推 **GitHub 一键登录**；也可用邮箱）
- 想解锁更多后缀配额，需要给其 GitHub 项目点 Star

## 逐步骤申请流程
1. 打开申请页：<https://domain.stackryze.com/>，点 **Get Started / Login**。
2. 用 **GitHub 登录**（或邮箱注册）。新账号初始只能申请 1 个 `indevs.in` 域名。
3. 在首页输入框查重，选一个可用前缀 + 后缀，点 **Check Availability → Claim** 即可秒开。
4. **扩容技巧**：在后台绑定 GitHub 并给 `stackryze/FreeDomains` 点 Star，可一次性解锁 `indevs.in`、`sryze.cc`、`ryzedns.org`、`nx.kg` 各 1 个配额。
5. 进 **Dashboard** 管理 DNS 记录（A / CNAME / MX / TXT / SVCB / HTTPS / TLSA 都支持）。

## 配置 DNS / 指向自己的站点
- 默认用 Stackryze 全球 NS：`ns1.stackryze.com`（纽约）、`ns2.stackryze.com`（纽伦堡）、`ns3.stackryze.com`（海得拉巴），在面板里直接改记录即可。
- 想套 Cloudflare：`indevs.in`、`sryze.cc` 实测可直接在 CF 添加并改用 CF 的 NS，证书自动续期。改 NS 后等生效（通常几分钟到几小时）即全盘接管。

## 注意事项与常见坑
- **有效期 1 年**：剩余不足 60 天时后台出现续期按钮，点一下继续免费，记得按时续。
- `*.nx.kg` 后缀经常因"请求过多"注册失败，建议优先抢注前三个后缀。
- 永久免费的可靠性取决于这家小团队能否持续运营——目前 4.6 万+ 域名、2.5 万+ 用户，但仍是社区项目，长期存在变数，重要业务别只押它一个。
- 有 Abuse 团队自动风控，钓鱼/垃圾内容会被封号。

## 参考链接
- <https://domain.stackryze.com/>
- <https://github.com/stackryze/FreeDomains>
- <https://www.ifeed.cc/articles/a3b369b8-f283-4c36-a90b-4b7e2be9f2ac>
- <https://stackryze.com/>
