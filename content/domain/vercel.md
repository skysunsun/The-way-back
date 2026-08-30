---
title: "Vercel 免费子域名申请与部署教程"
date: 2026-08-30
draft: false
tags: ["免费域名", "部署"]
math: false
---

## 服务简介

Vercel 是 Next.js 背后的团队做的部署平台，主打前端/静态站点一键上线。注册后会自动拿到一个 `*.vercel.app` 的子域名（例如 `my-app.vercel.app`），**完全免费、无需绑定信用卡**。我自己的个人项目和作品集都长期跑在它的免费 Hobby 套餐上，足够个人使用：每月 100GB 带宽、6000 分钟构建时长、自动 HTTPS、全球 CDN，还支持无限预览部署。

## 申请前置条件

- 一个 GitHub / GitLab / Bitbucket 账号（推荐 GitHub，授权最顺滑）
- 一个已经推送到仓库的前端项目（React、Vue、Next.js 甚至纯 HTML 都行）
- 能正常访问 vercel.com 的网络环境

## 逐步骤开通流程

1. 打开 [vercel.com](https://vercel.com)，点右上角 **Sign Up**，选 **Continue with GitHub** 授权登录。全程不要求填信用卡。
2. 登录后进入 Dashboard，点 **Add New → Project**。
3. 在仓库列表里找到你的项目，点 **Import**。第一次需要 GitHub 授权，按提示点 Authorize 即可，Vercel 只会读取你主动导入的仓库。
4. 进入配置页，Vercel 会读取 `package.json` 自动识别框架（Next.js / Vite / CRA 等），并自动填好 Build Command 和 Output Directory。若代码在子目录，记得设置 **Root Directory**。
5. 确认无误后点底部 **Deploy**。我会习惯在本地先跑一次 `npm run build`，避免把本来就坏的构建推上去。
6. 等 1–5 分钟，构建成功后会得到一个 `https://<项目名>.vercel.app` 的链接，直接打开就能访问。之后每次 `git push` 到主分支都会自动重新部署，体验非常爽。

## 绑定自定义域名 + 自动 HTTPS

1. 进入项目 **Settings → Domains**，输入你已购买的域名（如 `blog.example.com`），点 Add。
2. Vercel 会给出对应的 DNS 记录：
   - 子域名（如 `blog`）：添加 **CNAME** 记录，值填 `cname.vercel-dns.com`
   - 根域名（如 `example.com`）：添加 **A** 记录，值填 `76.76.21.21`
3. 去你的域名服务商（腾讯云、Cloudflare、阿里云等）后台添加对应记录，等几分钟全球生效。
4. Vercel 会自动签发 Let's Encrypt 证书，**免费 HTTPS + 强制跳转**全自动完成，无需任何额外操作。

## 注意事项与常见坑

- **构建超时**：Hobby 套餐单次构建上限 45 分钟，超出会失败，可用 `.vercelignore` 排除无用文件。
- **404 全页空白**：多半是 Output Directory 和实际产物目录不一致，确认 Vite 是 `dist`、Next 是 `.next`。
- **依赖缺失**：`Module not found` 往往是依赖只写在 `devDependencies` 或没提交 `package-lock.json`，本地先 `npm install` 跑通再推。
- **环境变量**：生产环境读不到变量，多半是部署后才添加，触发一次 Redeploy 即可。
- 免费版不支持团队多人协作（Pro 才有），高流量站点会受带宽限制。

## 参考链接

- [Vercel 官方部署文档](https://vercel.com/docs/deployments)
- [Vercel 定价（Hobby 免费层）](https://vercel.com/pricing)
- [前端部署全攻略（掘金）](https://juejin.cn/post/7591309945511165952)
