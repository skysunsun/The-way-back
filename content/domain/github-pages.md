---
title: "GitHub Pages 免费子域名与自定义域名绑定教程"
date: 2026-08-30
draft: false
tags: ["免费域名", "部署"]
math: false
---

## 服务简介

GitHub Pages 是 GitHub 官方提供的静态站点托管服务。只要你有 GitHub 账号，就能免费拿到一个 `*.github.io` 的子域名，还自带全球 CDN 和 HTTPS。**完全免费、不需要信用卡**，公开仓库和私有仓库都能开启（私有仓库开的站点内容依然是公开可访问的）。

来时路自己这个站，最早就是挂在 GitHub Pages 上，再把 dpdns.org 的免费域名指过去的。

## 前置条件

- 一个 GitHub 账号（[github.com](https://github.com/) 注册即可）
- 一个仓库，里面至少有 `index.html`，或者是 Hugo / Jekyll / Astro 这类静态站点的源码
- 想绑自定义域名的话，还要能改 DNS（比如 dpdns.org 的面板，或 Cloudflare DNS）

## 逐步开通流程

### 1. 建仓库

有两种站点类型，先想清楚要哪种：

- **用户页**：仓库名必须叫 `<你的用户名>.github.io`，站点地址是 `https://用户名.github.io/`，路径在根目录。
- **项目页**：仓库名随便取，站点地址是 `https://用户名.github.io/仓库名/`，多了一级子路径。

建议博客用**用户页**，省掉后面一堆路径问题。

### 2. 推代码

```bash
git init
git add .
git commit -m "init"
git branch -M main
git remote add origin https://github.com/你的用户名/你的用户名.github.io.git
git push -u origin main
```

### 3. 开启 Pages

进仓库 → **Settings** → 左侧边栏 **Pages**（在 "Code, planning, and automation" 分组里）。

- 纯 HTML 站：**Source** 选 `Deploy from a branch`，分支选 `main`，目录选 `/(root)`，Save。
- 需要构建的（Hugo / Astro / Vite 等）：**Source** 选 `GitHub Actions`，GitHub 会推荐对应框架的 workflow 模板，选一个套用即可。

第一次发布要等 1～10 分钟，成功后 Pages 设置页顶部会显示站点地址。

## 绑定自定义域名 + 自动 HTTPS

这一步是重点，来时路用的就是这个方案。

### 1. 在 GitHub 填域名

Settings → Pages → **Custom domain**，填入你的域名（如 `laishilu.dpdns.org`），Save。

注意：如果你的发布源是**分支**，GitHub 会自动在源分支根目录提交一个 `CNAME` 文件；如果发布源是 **GitHub Actions**，则不会创建 `CNAME` 文件，仓库里的 `CNAME` 也会被忽略、不再需要。

### 2. 配 DNS 记录

**子域名**（推荐，如 `blog.example.com`）——加一条 CNAME：

| 类型 | 名称 | 值 |
|---|---|---|
| CNAME | blog | `你的用户名.github.io` |

注意 CNAME 的值只写到 `用户名.github.io`，**不要带仓库名**。

**根域名**（apex，如 `example.com`）——加 4 条 A 记录：

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

想支持 IPv6 再补 4 条 AAAA：

```
2606:50c0:8000::153
2606:50c0:8001::153
2606:50c0:8002::153
2606:50c0:8003::153
```

如果 DNS 商支持 `ALIAS` / `ANAME`，也可以直接把 `@` 指向 `用户名.github.io`。

### 3. 开 HTTPS

DNS 校验通过后（Pages 设置页出现绿色对勾），勾上 **Enforce HTTPS**。GitHub 会自动通过 Let's Encrypt 签发并续期证书，全程免费。官方说明这个选项**最多需要 24 小时才会变可用**，通常十几分钟到一两小时就好了。灰着点不动就是 DNS 还没生效，等等再刷新。

验证一下：

```bash
curl -I https://blog.example.com   # 期望 HTTP/2 200
dig blog.example.com +noall +answer
```

## 注意事项与常见坑

1. **项目页的相对路径**：项目页站点在 `/仓库名/` 子目录下，HTML 里写 `/about.html` 这种绝对路径会 404，要么改成相对路径，要么在静态生成器里设好 `baseURL`。
2. **CNAME 文件别手删**：分支发布模式下，删掉 `CNAME` 等于取消自定义域名。本地构建后推送的话，记得先 `git pull` 把 GitHub 自动生成的这个提交拉下来，否则下次推送会把它覆盖掉。
3. **用 Actions 部署要带上 cname**：例如 `peaceiris/actions-gh-pages` 要加 `cname: your.domain`，不然每次部署都会丢。
4. **Jekyll 的坑**：纯静态站或框架产物如果 CSS/下划线开头的目录加载不出来，在发布目录根加一个空的 `.nojekyll` 文件。
5. **不要用通配符 DNS**：官方明确警告 `*.example.com` 这类记录有子域名被接管的风险。
6. **DNS 生效时间**：通常 15 分钟内，最坏 24～48 小时。别急着反复改，改来改去反而更慢。
7. **CAA 记录**：如果你给域名设过 CAA，要确保放行 `letsencrypt.org`，否则证书签不出来。
8. **别把子域名 CNAME 指向自己的根域名**，官方说这会导致 HTTPS 强制失败、甚至根本访问不到站点。

## 参考链接

- [Managing a custom domain for your GitHub Pages site - GitHub Docs](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site)
- [Securing your GitHub Pages site with HTTPS - GitHub Docs](https://docs.github.com/en/pages/getting-started-with-github-pages/securing-your-github-pages-site-with-https)
- [Configuring a publishing source for your GitHub Pages site](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site)
- [How to Set Up a GitHub Pages Blog with Custom Domain — 2026 Guide](https://dev.to/profiterole/how-to-set-up-a-github-pages-blog-with-custom-domain-complete-2026-guide-4c27)
