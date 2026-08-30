---
title: "Render 免费子域名申请与部署教程"
date: 2026-08-30
draft: false
tags: ["免费域名", "部署"]
math: false
---

## 服务简介

Render 是被称为「Heroku 替代品」的 Git 化云平台，能部署 Web 服务、静态站、数据库和定时任务。注册即获一个 `*.onrender.com` 的专属子域名（例如 `my-api.onrender.com`）。**免费层（官方称 Free / Hobby）无需信用卡**，我实测用 GitHub 直接注册就能部署。免费额度：Web 服务每月 750 实例小时、静态站无限且常驻、100GB 带宽、500 构建分钟、0.1 CPU / 512MB 内存，还自带自动 HTTPS，非常适合个人后端 API 和演示项目。

## 申请前置条件

- 一个 GitHub / GitLab / Bitbucket 账号（也可邮箱注册）
- 一个已推送的仓库：含服务端代码的用 Web Service，纯 HTML/CSS/JS 的用 Static Site
- 注意运行时读取端口环境变量 `$PORT`，别写死端口

## 逐步骤开通流程

1. 打开 [render.com](https://render.com)，点 **Sign Up**，用 GitHub 授权登录。**免费层不要求信用卡**。
2. 登录后进入 Dashboard，点左上角 **New +**，按项目类型选 **Web Service**（有服务端代码）或 **Static Site**（纯静态）。
3. 在 **Git Provider** 下点 **Add credential** 授权你的代码托管平台，然后选中要部署的仓库。
4. 配置服务：填写 **Name**（决定 `onrender.com` 前缀）、**Region**、**Branch**、运行时（Node/Python 等会自动识别）。
   - Web Service 需设 **Build Command**（如 `pip install -r requirements.txt` 或 `npm install`）和 **Start Command**（如 `uvicorn main:app --host 0.0.0.0 --port $PORT`）。
   - **关键**：启动命令务必用 `$PORT`，Render 会注入真实端口。
5. **Instance Type** 选 **Free**，点 **Create Web Service / Deploy**。
6. 等 2–3 分钟构建完成，在服务页面顶部即可看到 `https://<名称>.onrender.com`，自动带 HTTPS，直接打开访问。之后每次 `git push` 自动重新部署。也可以用 CLI（`render login` + `render services create`）或提交 `render.yaml` 走 Blueprint。

## 绑定自定义域名 + 自动 HTTPS

1. 进入服务页面 **Settings → Custom Domains**，输入你自己的域名，点 **Add**。
2. 按提示在域名服务商处添加 **CNAME** 记录指向 Render 给出的地址（根域名用 A 记录指向 Render 提供的 IP）。
3. Render 会**自动签发并托管 TLS 证书**，HTTPS 全免费、自动续期，无需任何额外操作。免费层同样支持自定义域名。

## 注意事项与常见坑

- **休眠策略（最大坑）**：免费 Web 服务闲置 15 分钟会自动休眠，下次请求需冷启动约 1 分钟，首个访客会等很久。低流量演示可接受；要常驻需升级付费实例。静态站则**永不休眠**。
- **750 小时账本**：额度是按整个 workspace 算的，一个 24/7 服务约耗 720 小时，基本刚好用满；超额后该月所有免费服务被暂停。
- **免费 Postgres 30 天过期**：免费数据库创建 30 天后失效，有 14 天宽限期，长期数据请升级或换 Supabase / Neon。
- **临时文件系统**：运行时写入的本地文件在休眠/重部署后全丢失，持久数据请放数据库或外部对象存储。
- **端口与 SMTP**：必须用 `$PORT`；免费层屏蔽 25/465/587 发信端口，邮件需走第三方服务。

## 参考链接

- [Render 官方「Your First Deploy」](https://docs.render.com/docs/your-first-deploy)
- [Render 免费层官方文档](https://render.com/docs/free)
- [Render Free Tier 2026 详解](https://deploybase.app/blog/render-free-tier-complete-guide-2026)
