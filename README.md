# skysun 的来时路

> 记录与分享我走过的学术、技术与投资之路。

一个**纯静态、零服务器依赖**的博客：用 [Hugo](https://gohugo.io) 生成，托管在
GitHub Pages，绑定自定义域名 **sky.dpdns.org**。

内容分三类：

- **研究** (`/research/`)：行人过街意图预测、频率域特征工程、论文写作
- **技术** (`/tech/`)：Python、数据可视化、工程实践
- **投资** (`/investment/`)：期货交易、ETF 定投、量化筛选复盘

---

## 技术栈与架构

- **内容**：Markdown，放在 `content/` 下，按栏目分目录
- **构建**：Hugo（extended）把 Markdown 渲染成静态站
- **部署**：推送 `main` → GitHub Actions（`peaceiris/actions-hugo` + `peaceiris/actions-gh-pages`）
  构建并发布到 `gh-pages` 分支 → GitHub Pages 对外服务
- **域名**：自定义域名 `sky.dpdns.org`（通过仓库里的 `static/CNAME` 锁定）
- **公式 / 代码**：数学公式用 KaTeX（文章 front matter 设 `math: true` 才加载），代码高亮内置

---

## 1. 本地预览

```bash
# 先装 Hugo extended：https://gohugo.io/install/
hugo server -b http://localhost:1313/
# 浏览器打开 http://localhost:1313/
```

> 因为线上用自定义域名（根路径），本地预览**必须带** `-b http://localhost:1313/`，
> 否则文章链接会带 `/The-way-back/` 前缀而 404。

---

## 2. 写一篇新文章

```bash
hugo new research/我的文章.md     # 自动套用 archetypes/default.md 模板
```

Front Matter 字段：

```yaml
---
title: "标题"
date: 2026-08-30
draft: true          # 草稿：true 时不发布，写好改 false
tags: ["标签1", "标签2"]
math: false          # 需要数学公式（KaTeX）时设 true
---
```

- 正文用 Markdown，支持代码块、表格、`> 引用`、图片
- 想手动控制首页摘要，正文里插 `<!--more-->`
- 文章放 `content/<栏目>/` 下即被对应栏目收录（`research` / `tech` / `investment`）

---

## 3. 部署（已配好，日常只管 push）

仓库已建好：`github.com/skysunsun/The-way-back`，本地 `origin` 已指向它。

日常更新只需：

```bash
git add -A
git commit -m "更新说明"
git push
```

推送后 GitHub Actions 自动构建并发布，约 1 分钟生效。

> 首次上线还需在 GitHub 后台做**一次**设置（见第 4、5 节），之后就不用管了。

---

## 4. 首次上线：开启 GitHub Pages

仓库 **Settings → Pages → Build and deployment → Source** 选：

- **Deploy from a branch**
- Branch：**`gh-pages`**，目录：**`/ (root)`**
- 保存

---

## 5. 自定义域名 sky.dpdns.org

仓库里已放 `static/CNAME`（内容 `sky.dpdns.org`），每次部署自动带上去，
GitHub 据此锁定域名。你还需：

1. **DNS**：在域名服务商处给 `sky.dpdns.org` 加一条 **CNAME 记录**，指向
   `skysunsun.github.io.`（推荐，因为是子域名）。
   若只能加 A 记录，则指向 GitHub Pages 四个 IP：
   `185.199.108.153` / `109` / `110` / `111.153`。
2. **GitHub 设置**：Settings → Pages → Custom domain 填 `sky.dpdns.org` 并保存。
   GitHub 会自动申请并续期 HTTPS 证书（等 DNS 解析生效，几分钟到几小时），
   证书就绪后勾选 **Enforce HTTPS**。

上线后访问：**https://sky.dpdns.org/**

> 注意：绑定后 `sky.dpdns.org` 用于访问博客，不再指向你那台 1c1g 服务器；
> 若服务器还要用这个域名，请给它另起子域（如 `server.dpdns.org`）。

---

## 6. （可选）以后用 1c1g 做国内镜像

国内访问 GitHub Pages 偶尔偏慢，等需要时可在 1c1g 上：

- 用 `nginx` / `Caddy` 反代 `https://sky.dpdns.org/`（或 `https://skysunsun.github.io/The-way-back/`）；
- 或 `rsync` 拉取 `public/` 做静态托管 + Caddy 自动 HTTPS；
- 再叠加国内 CDN 进一步加速。

1c1g（1 核 1G）跑静态站绰绰有余，跑不动 WordPress 这类动态 CMS，所以静态方案最稳。

---

## 目录结构

```
blog/
├── config.toml              # 站点配置（标题/菜单/描述/baseURL）
├── archetypes/default.md    # 新文章模板
├── layouts/                 # 页面模板（自写，无第三方主题依赖）
│   ├── _default/{baseof,single,list}.html
│   ├── index.html           # 首页
│   └── partials/            # head / header / footer / post-card
├── static/
│   ├── css/style.css        # 样式（简洁响应式）
│   └── CNAME                # 自定义域名（sky.dpdns.org）
├── content/                 # 文章（research / tech / investment / about）
└── .github/workflows/       # 自动部署（deploy.yml）
```
