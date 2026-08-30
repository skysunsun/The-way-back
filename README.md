# skysun 的来时路

记录与分享我走过的学术、技术与投资之路。

纯静态博客（Hugo + GitHub Pages），绑定域名 **sky.dpdns.org**。内容分三类：
**研究** `/research/`、`/tech/`、`/investment/`。

## 本地预览
```bash
hugo server -b http://localhost:1313/
```

## 写新文章
```bash
hugo new research/标题.md   # 放 content/<栏目>/ 下即被收录
```
Front matter 支持 `draft`（草稿）、`tags`、`math: true`（加载 KaTeX 公式）。

## 更新上线
```bash
git add -A && git commit -m "说明" && git push
```
推送后 GitHub Actions 自动构建发布（`gh-pages` 分支）。

> 首次上线需到仓库 **Settings → Pages → Source** 选 `Deploy from a branch` / `gh-pages` / `root`；
> 自定义域名 `sky.dpdns.org` 在 DNS 加 CNAME 指向 `skysunsun.github.io.` 后，于 Pages → Custom domain 填写。
