---
title: "InfinityFree 免费主机与子域名申请教程"
date: 2026-08-30
draft: false
tags: ["免费域名", "免费主机"]
math: false
---

# InfinityFree 免费主机与子域名申请教程

去年我给一个开源小工具做演示页，不想花钱买服务器，于是踩进了 InfinityFree 这个坑——结果它真的能白嫖，至今没被收过一分钱。下面是我实测可用的当前流程（核实于 2026-08）。

## 服务简介
InfinityFree 是一家运营了 14 年以上的免费虚拟主机商，提供 PHP、MySQL 和免费 SSL。它本身**不卖域名**，但会在免费主机套餐里自动分配一个永久有效的子域名。可选后缀包括 `*.rf.gd`、`*.epizy.com`、`*.great-site.net` 等 25+ 种。完全**免费、无需绑卡、无广告、无到期日**。

## 申请前置条件
- 一个能收验证码/通知的邮箱
- 无需信用卡、无需实名、无需备案

## 逐步骤申请流程
1. 打开注册页：<https://dash.infinityfree.com/register>（官网入口在 infinityfree.net 的 "Register Now"）。
2. 填写邮箱、用户名、密码，完成人机验证，提交注册。
3. 邮箱激活后登录控制面板。在 **Accounts → Create Account** 里新建一个站点，系统会让你选一个免费子域名（如 `mysite.rf.gd` 或 `mysite.epizy.com`），确认即可。
4. 账户**分钟级自动开通**，无审核、无等待名单。
5. 进入面板后，用 **Online File Manager** 把网页传到 `htdocs/`（旧版叫 `public_html/`），或用 **Softaculous** 一键装 WordPress。
6. 子域名自动解析，Let's Encrypt 证书自动签发，HTTPS 即时可用。

## 配置 DNS / 指向自己的站点
- 想绑自己的域名：面板里 **Add Domain** 添加已在别处注册好的域名，按提示把 NS 或 A 记录指到 InfinityFree，并做 CNAME/验证记录。
- 自带 **Free DNS Service**，可在面板里直接改 A、CNAME、MX、TXT 等记录。

## 注意事项与常见坑
- 免费档标称 **5 GB 磁盘、无限带宽、PHP 8.3、MySQL 8.0**，但官方对**单账户文件数（约 10 万）和每天访问量（约 5 万 hits）有软上限**，超额会限流。
- 不允许放盗版、成人、钓鱼等内容，违规会被停号。
- "无限带宽"指不单独计费，但异常流量仍会被风控。
- 子域名长期不用有被回收风险，建议保持活跃。
- 它只给主机+子域名，顶级域名还需另购；需要稳定生产环境仍建议升级付费档或自购域名。

## 参考链接
- <https://infinityfree.net/>
- <https://dash.infinityfree.com/register>
- <https://www.php.cn/faq/2083852.html>
