# 孙博士的教程博客（Hugo + GitHub Pages）

一个**纯静态、零服务器依赖**的教程博客，内容分三类：

- **研究** (`/research/`)：行人过街意图预测、频率域特征工程、论文写作笔记
- **技术** (`/tech/`)：Python、数据可视化、工程实践
- **投资** (`/investment/`)：期货交易、ETF 定投、量化筛选复盘

构建与部署全自动：推送到 `main` 分支 → GitHub Actions 用 Hugo 生成静态站 →
发布到 GitHub Pages。你那台 1c1g 服务器先不用，留作以后国内镜像。

---

## 1. 本地预览

```bash
# 安装 Hugo extended（Win/Mac/Linux 均可，https://gohugo.io/install/）
hugo server -b http://localhost:1313/   # 注意带 -b，链接才会正确
# 浏览器打开 http://localhost:1313/
```

> 因为本站部署在 GitHub 项目子路径下（`/blog/`），本地预览务必加
> `-b http://localhost:1313/`，否则文章链接会带 `/blog/` 前缀而 404。

## 2. 写一篇新文章

```bash
hugo new research/my-post.md     # 自动套用 archetypes/default.md 模板
```

模板 Front Matter 字段说明：

```yaml
---
title: "标题"
date: 2026-08-30
draft: true          # 草稿：true 时不会发布，写好改 false
tags: ["标签1", "标签2"]
math: false          # 需要数学公式（KaTeX）时设 true，会自动加载依赖
---
```

- 正文用 Markdown 写，支持代码块、表格、`> 引用`、图片。
- 想手动控制首页摘要，在正文中插入 `<!--more-->`。
- 文章放在 `content/<分区>/` 下即可被对应栏目收录。

## 3. 部署到 GitHub Pages

1. 在 GitHub 新建一个仓库，例如 `blog`（仓库名会变成网址里的子路径）。
2. 把本目录推上去：

   ```bash
   git init
   git add .
   git commit -m "init blog"
   git branch -M main
   git remote add origin git@github.com:你的用户名/blog.git
   git push -u origin main
   ```

3. 仓库 **Settings → Pages → Build and deployment → Source 选 "Deploy from a branch"**，
   分支选 **`gh-pages`**、目录选 **`/ (root)`**，保存。
4. 等 Actions 跑完（约 1 分钟），访问 `https://你的用户名.github.io/仓库名/`。

> 之后你只需要 `git push`，站点会自动更新。

## 4. 改成你自己的信息

- `config.toml`：改 `title`、`params.author`、`params.github`、`baseURL`。
- `content/about.md`：改自我介绍。
- 示例文章在 `content/research|tech|investment/` 下，可删可改。

## 5. （可选）绑定自定义域名

1. 域名 DNS 添加 CNAME 指向 `你的用户名.github.io.`。
2. 仓库 Settings → Pages → Custom domain 填入你的域名，GitHub 会自动签 HTTPS 证书。
3. 在 `static/` 下放一个 `CNAME` 文件（内容只有你的域名），避免每次部署被清空。

## 6. （可选）以后用 1c1g 做国内镜像

国内访问 GitHub Pages 有时偏慢。等你需要时，可以：

- 在 1c1g 上用 `nginx` 或 `Caddy` 反代 `https://你的用户名.github.io/blog/`；
- 或在服务器上 `rsync` 拉取 `public/` 目录做静态托管 + Caddy 自动 HTTPS；
- 用自定义域名 + 国内 CDN 进一步加速。

1c1g（1 核 1G）跑静态站完全够，跑不动 WordPress 这类动态 CMS，所以静态方案最稳。

---

## 目录结构

```
blog/
├── config.toml              # 站点配置（标题/菜单/渲染）
├── archetypes/default.md    # 新文章模板
├── layouts/                 # 页面模板（无需第三方主题）
│   ├── _default/{baseof,single,list}.html
│   ├── index.html           # 首页
│   └── partials/            # head/header/footer/post-card
├── static/css/style.css     # 样式（简洁响应式）
├── content/                 # 文章（research/tech/investment/about）
└── .github/workflows/       # 自动部署
```
