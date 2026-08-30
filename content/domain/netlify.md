---
title: "Netlify 免费子域名申请与部署教程"
date: 2026-08-30
draft: false
tags: ["免费域名", "部署"]
math: false
---

## 服务简介

Netlify 是老牌的现代化前端托管平台，注册即送一个 `*.netlify.app` 的随机子域名（例如 `random-name-12345.netlify.app`）。**免费、无需信用卡**。我第一次把 React 应用拖上去就拿到公网链接，前后不到 5 分钟。免费套餐包含：全球 CDN、自定义域名 + 免费 SSL、Serverless 函数、无限预览部署，每月 100GB 带宽、300 分钟构建时长，对个人项目绰绰有余。

## 申请前置条件

- 一个免费 Netlify 账号（可用 GitHub 直接登录）
- 一个已完成的项目：可以是已推到 GitHub 的仓库，也可以只是本地一个构建好的文件夹
- 想自动部署才需要 GitHub 账号；只想快速上线，本地文件夹就够了

## 逐步骤开通流程

Netlify 提供三种方式，我用得最多的是前两种。

**方式一：连接 Git 自动部署（推荐）**

1. 打开 [netlify.com](https://netlify.com) 注册，选 GitHub 登录，无需信用卡。
2. 进入 Dashboard，点 **Add new project → Import an existing project**，选 GitHub。
3. 授权 Netlify 读取仓库（只读，安全），从列表选你的仓库。
4. 配置构建：**Build command**（如 `npm run build`）、**Publish directory**（如 `dist`/`build`）。纯静态站可不填。确认部署分支默认是 `main`。
5. 点 **Deploy**，等 30–60 秒，拿到 `https://<随机名>.netlify.app` 链接。之后每次 `git push` 自动重建上线。

**方式二：拖拽部署（最快）**

1. 本地先 `npm run build` 生成产物文件夹。
2. 打开 [app.netlify.com/drop](https://app.netlify.com/drop)，把文件夹或 zip 直接拖进上传区。
3. 几秒后得到一个 `netlify.app` 链接，立即可访问。缺点是不带自动重部署，更新需重新拖一次。

**方式三：CLI 部署**，适合习惯命令行的同学：`npm i -g netlify-cli` 后 `netlify deploy --prod`。

## 绑定自定义域名 + 自动 HTTPS

1. 在 Dashboard 进入站点 **Domain management → Add a domain**，输入你的域名。
2. Netlify 提供两种接法：
   - **推荐用 Netlify 名称服务器**：把域名的 NS 改为 `dns1.p01.nsone.net` 等四条，DNS 全权交给 Netlify，最省心。
   - 或手动加 **CNAME** 记录指向 Netlify 给你的地址。
3. 配置完成后，Netlify **自动签发并续期 SSL 证书**，HTTPS 免费且强制跳转，无需任何手动步骤。

想改掉随机子域名？进 **Site settings → Change site name** 即可自定义 `xxx.netlify.app` 前缀。

## 注意事项与常见坑

- **空白页**：多半没设 `homepage` 或资源用了绝对路径，改用相对路径 `./` 即可。
- **SPA 路由 404**：在 `public/` 下加 `_redirects` 文件，内容 `/* /index.html 200`。
- **计费模式**：Netlify 用「信用点」计费，免费套餐是**硬上限**，超额只会暂停而非扣费，不会收到意外账单，这点很友好。
- **构建时长**：免费版每月仅 300 分钟，频繁构建要注意；先在本地跑通构建再推。
- 免费静态站**不会休眠**（休眠是 Render 的特性），适合长期挂个人站。

## 参考链接

- [Netlify Drop 快速开始（官方文档）](https://docs.netlify.com/start/get-started-with-drop)
- [Netlify 部署指南 2026](https://aisnag.ai/how-to-deploy-on-netlify/)
- [Netlify 免费部署 React 实战](https://dev.to/sohanaakbar7/how-to-deploy-react-app-on-netlify-for-free-5-minutes-2jk7)
