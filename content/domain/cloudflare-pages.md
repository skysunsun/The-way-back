---
title: "Cloudflare Pages 免费子域名与自定义域名绑定教程"
date: 2026-08-30
draft: false
tags: ["免费域名", "部署"]
math: false
---

## 服务简介

Cloudflare Pages 是 Cloudflare 提供的静态站点托管服务。连上 Git 仓库部署成功后，会免费送你一个 `<项目名>.pages.dev` 的子域名，自带 330+ 城市的全球 CDN 和自动 HTTPS。**免费、不需要信用卡。**

免费额度相当阔气：**带宽不限量**、每月 500 次构建、100 个项目、每项目 100 个自定义域名、预览部署无限。带宽不限量这点是它比 Netlify / Vercel（各 100GB/月）最舒服的地方——文章万一被转爆了也不会突然收你钱。

来时路对比过一圈，静态博客最后就落在这套方案上：Cloudflare Pages 部署 + Cloudflare DNS 管域名，全链路一家搞定。

## 前置条件

- Cloudflare 账号（[dash.cloudflare.com](https://dash.cloudflare.com/) 免费注册）
- GitHub 或 GitLab 账号，代码已推到仓库（公开、私有都支持）
- GitLab 用户需要在该仓库拥有 **Maintainer** 或更高角色
- 想绑根域名的话，域名要作为 zone 托管在你这个 Cloudflare 账号下

## 逐步开通流程

### 1. 先把代码推上 Git

以 Hugo 为例，先写 `.gitignore` 避免把构建产物提交进去：

```
/public/
/resources/_gen/
.hugo_build.lock
```

然后推送：

```bash
git init
git add .
git commit -m "init"
git branch -M main
git remote add origin https://github.com/你的账号/你的仓库.git
git push -u origin main
```

### 2. 连接 Git 仓库

登录 Cloudflare 控制台 → 左侧 **Workers & Pages** → **Create application** → **Pages** → **Connect to Git**。

会弹出 Git 提供商授权，点 **Install & Authorize**，选中你的仓库，再点 **Begin setup**。

> 注意：一旦选了 Git 集成模式，**后面不能再切回 Direct Upload**（直接上传）模式。想反过来的话只能新建项目。

### 3. 配置构建

在 "Set up builds and deployments" 里填：

- **Project name**：决定你的 `xxx.pages.dev` 前缀
- **Production branch**：一般是 `main`，其他分支会自动变成预览部署
- **Framework preset**：选对应框架，会自动填好命令；纯 HTML 就留空

常见框架的配置：

| 框架 | 构建命令 | 输出目录 |
|---|---|---|
| Hugo | `hugo --gc --minify` | `public` |
| Astro / Starlight | `npm run build` | `dist` |
| MkDocs | `pip install -r requirements.txt && mkdocs build` | `site` |
| 纯 HTML | 留空 | `/`（或你的目录） |

**环境变量**建议顺手设上，锁死版本，避免本地能跑线上炸：

- Hugo：`HUGO_VERSION = 0.140.0`（跟本地一致；Hextra 等主题需要 extended 版，Cloudflare 会按你指定的版本装 extended）
- Node 项目：`NODE_VERSION = 20`
- Python 项目：`PYTHON_VERSION = 3.12`

单仓库多项目（monorepo）的话，用 **Root directory (advanced) → Path** 指定子目录。

### 4. 部署

点 **Save and Deploy**，构建日志实时刷出来。第一次一般 1～3 分钟。完成后就能拿到 `项目名.pages.dev` 地址。

**之后每次 push 到 main 都会自动重新构建部署**，其他分支和 PR 会生成独立的预览地址，非常适合改版前先看效果。

## 绑定自定义域名 + 自动 HTTPS

### 操作路径

控制台 → **Workers & Pages** → 选中你的 Pages 项目 → **Custom domains** → **Set up a domain** → 填入域名 → **Continue**。

### 两种情况

**域名已托管在 Cloudflare**（推荐）：确认一下，CNAME 记录会**自动帮你加好**，基本一键完事。

**域名在别处托管**：只有子域名能这么玩。去你的 DNS 商那里手动加：

| 类型 | 名称 | 内容 |
|---|---|---|
| CNAME | blog | `你的项目名.pages.dev` |

**绑根域名（apex，如 example.com）**：必须先把域名作为 zone 加进 Cloudflare，并把 NS 服务器改成 Cloudflare 的。因为 Cloudflare DNS 支持 CNAME flattening，根域名也能直接 CNAME 到 `pages.dev`，这是最干净的做法。

顺便说，Cloudflare DNS 本身是**免费**的，把域名 NS 转过来托管不花钱，还能顺便用上它的缓存和防护。

### HTTPS

连接完成后，**Cloudflare 会自动签发并自动续期 HTTPS 证书**，不用你管，也没有 "Enforce HTTPS" 之类的开关要勾。这块比 GitHub Pages 省心。

## 注意事项与常见坑

1. **别跳过控制台直接加 CNAME**。官方明确说：不先在 Pages 项目里 **Set up a domain**、只在 DNS 里手动加 CNAME 指过去，域名会解析失败并报 **522 错误**。顺序一定是先控制台、后 DNS。
2. **首次构建失败九成是环境问题**。Hugo 版本、Node 版本对不上是最常见原因，用环境变量锁版本解决。Hugo Module 拉不下来可以补 `GO_VERSION`。
3. **文件数限制**：单次部署最多 20,000 个文件（免费版），单文件最大 25 MiB。Next.js 图片优化导出很容易爆这个数，超了直接部署失败，得精简产物或把大资源挪到 R2 / 外部 CDN。
4. **Hugo 未来日期文章不会自动发布**。Hugo 默认排除未来日期的文章，而 Pages 只在 push 时才构建——到了那天没人触发构建，文章就一直不出来。要定时发布得配每日触发的 deploy hook，不然干脆写完就用当天日期。
5. **CAA 记录**要放行 `letsencrypt.org`、`pki.goog`、`ssl.com`，否则证书签不出来。
6. **DNS 记录改走别处再改回来**，自定义域名会变成 inactive，恢复期间访客会看到报错。临时想切流量，用 Origin rule 或 Redirect rule，不要动 DNS。
7. **删项目前先删 CNAME**。带自定义域名的项目，要先删掉 DNS 里的 CNAME 记录再删项目，否则域名会悬空指向一个已不存在的项目。
8. **自定义域名生效后记得改站点配置**里的绝对地址（Hugo 的 `baseURL`、Astro 的 `site`），不然 sitemap 和站内搜索会指错。

## 参考链接

- [Git integration guide · Cloudflare Pages docs](https://developers.cloudflare.com/pages/get-started/git-integration/)
- [Custom domains · Cloudflare Pages docs](https://developers.cloudflare.com/pages/configuration/custom-domains/)
- [Build configuration · Cloudflare Pages docs](https://developers.cloudflare.com/pages/configuration/build-configuration/)
- [Deploy Static Sites to Cloudflare Pages (Free Hosting)](https://computingforgeeks.com/deploy-static-site-cloudflare-pages/)
