---
title: "pp.ua 免费域名申请教程"
date: 2026-08-30
draft: false
tags: ["免费域名", "部署"]
math: false
---

## 服务简介

`.pp.ua` 是乌克兰官方为个人提供的免费二级域名后缀（PP = Private Person），由 NIC.UA 运营，稳定性有保障。注册与续期**完全免费**，可用于个人博客、Demo、绑定 GitHub Pages / Cloudflare 等。域名长度 3–63 位，支持拉丁/西里尔字母甚至 emoji。Whois 联系人信息**公开不可隐藏**（域规要求）。

> 现状提醒（2026）：相比早年"纯邮箱即可"，现在增加了一道**实名激活**——需要 Telegram + 一张用于验证的 VISA 卡（约 1₴ 预授权，多数情况不真扣），且联系电话必须与 TG 绑定手机号一致。门槛比过去高，但仍是长期免费里最稳的之一。

## 申请前置条件

- 一个真实可用邮箱（收验证邮件）
- 一个 **Telegram 账号**（用于 @ppuabot 激活域名）
- 一个**手机号**，且必须和上面 Telegram 绑定的手机号一致
- 一张 **VISA 卡**（用于 Payment Cards 验证，建议国内 Visa 实体/虚拟卡均可试）

## 逐步骤申请流程（带邀请链接）

通过邀请链接注册：<https://nic.ua/en/signup/voavephe>

1. 用邮箱 + 密码注册，去邮箱点开 "Welcome to NIC.UA" 邮件完成验证。

   ![邀请注册页：填写 E-mail、Password，含 I have an invitation code（走邀请链接）](/img/tech/ppua-1.png)
2. 回到 <https://nic.ua/> 首页搜索框输入想要的域名（如 `myname.pp.ua`），点 **Search** 查是否可用。

   ![域名搜索结果：绿色勾 + available for registration，加入购物车后点 Checkout（Is free）](/img/tech/ppua-2.png)
3. 未被占用会出现绿色勾和 **Add to cart**，加入购物车后点 **Checkout** 去结算（免费，无需付款）。
4. 填写域名联系人信息（Contact）：
   - **电话务必填 Telegram 绑定的那个手机号**，填错将无法激活且难改；
   - 国家选 China，姓名/地址用大写拼音即可，邮箱务必正确。
   - 点 **Save contact** 保存。

   ![填写联系人界面（Private information of the account owner）：Phone number 务必填 Telegram 绑定的手机号](/img/tech/ppua-3.png)
5. 到 **Payment Cards** 添加一张 VISA 卡，会提示扣 1.00₴ 预授权（实测多不真扣），完成卡片验证。

   ![Payment Cards 绑 VISA 卡界面：左侧 Payment cards 入口，Add card 按钮（暂无卡片状态）](/img/tech/ppua-4.png)
6. 回到首页重新搜索该域名 → 加入购物车 → 结账，域名才会进入你的管理后台（否则仍会提示"需绑卡"）。

## 如何激活域名

1. Telegram 搜索 **@ppuabot**，发送 `/start`。

   ![Telegram @ppuabot /start：要求 Send phone number（按提示分享 Telegram 绑定的手机号）](/img/tech/ppua-5.png)
2. 点 **Activate domain** 按钮，按提示输入刚申请的域名，再确认 `yes`。

   ![@ppuabot 激活对话：输入域名 → 同意 PP.UA 政策 → Yes，机器人返回激活地址 https://apu.drs.ua](/img/tech/ppua-6.png)
3. 机器人会返回一串**激活码**和激活地址。
4. 打开 <https://apu.drs.ua> → 填入域名、TG 绑定的电话、激活码 → 激活成功。

   ![apu.drs.ua 激活页：Domain / Phone number / Key + Continue（输入激活码完成激活）](/img/tech/ppua-7.png)
   （也可在 https://pp.ua 用短信收到的域码激活，但 TG 方式更稳。）

## 如何配置 DNS / 指向自己的站点

`.pp.ua` 支持自定义 NS，可托管到 Cloudflare：

![NIC.UA 后台 Domains 域名列表（从这里进入单个域名的 NS 设置）](/img/tech/ppua-8.png)
1. Cloudflare → **添加站点** → 输入完整域名 `myname.pp.ua` → Free 计划。

   ![Cloudflare 名称服务器配置页：将当前 NS 替换为 Cloudflare 分配的两个名称服务器（piper/pranab.ns.cloudflare.com），删除旧 NS，保存并继续](/img/tech/ppua-9.png)
2. 复制 Cloudflare 分配的两个 NS，回 NIC.UA 后台把域名的 Name Server 改成这两个 → 保存。

   ![NIC.UA 后台改 NS：单域名详情页 NS-servers → Custom name servers，填入 Cloudflare 的两个 NS（megan/armando.ns.cloudflare.com）](/img/tech/ppua-10.png)
3. Cloudflare 侧检查 NS，变 **Active** 即托管完成，SSL 自动签发。
4. 添加记录：CNAME `@` → `yourname.github.io`（GitHub Pages 自定义域 + Enforce HTTPS）；或 A `@` → 服务器 IP。非 HTTP 服务记得把云朵点灰。

## 注意事项与常见坑

- **电话 = TG 手机号** 是铁律，不一致激活直接报错。
- **绑卡是必需步骤**，无卡基本走不通；预授权通常不真扣，但仍建议用可控的小额卡。
- **续期**：委托期 1 年，到期前 60 天内可免费续；过期有 28 天优先续期期，再之后恢复需付约 996₴ 且不保证成功。
- **30 天内最多注册 3 个** `.pp.ua`。
- **隐私**：Whois 公开联系人，介意手机号暴露可用 GV 之类号码。
- **稳定性**：官方运营、长期免费，比私人项目靠谱；国内访问建议走 Cloudflare。

## 参考链接

- 邀请注册：<https://nic.ua/en/signup/voavephe>
- 官方介绍页：<https://nic.ua/en/domains/.pp.ua>
- 激活地址：<https://apu.drs.ua>
- 社区实测（NodeSeek）：<https://www.nodeseek.com/post-273811-1>
