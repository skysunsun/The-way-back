---
title: "免费域名全汇总：9 个平台横向对比"
date: 2026-08-30
draft: false
tags: ["免费域名", "域名申请"]
math: false
---

之前写了三篇带邀请链接的详细教程（[DigitalPlat 的 qzz.io/dpdns.org](/tech/digitalplat-free-domain/)、[kdns.fr](/tech/kdns-fr/)、[pp.ua](/tech/pp-ua/)）。这篇把其余 9 个免费域名 / 子域名平台做个**汇总对比**，方便一眼挑。各平台都已联网核实 2026 年当前状态。

## 一表总览

| 平台 | 可获后缀 | 完全免费 | 需信用卡/手机 | 审核 | 适合 |
| --- | --- | --- | --- | --- | --- |
| EU.org | `*.eu.org` | 是 | 否 | 人工，数周~数月（不稳定） | 长期稳定的博客 / 自托管 |
| is-a.dev | `*.is-a.dev` | 是 | 否（GitHub 登录） | PR 审核 24–72h | 开发者作品集 / 技术站 |
| InfinityFree | `*.rf.gd` / `*.epizy.com` | 是（附 PHP 主机） | 否 | 即时 | 低流量 WordPress / 练手 |
| Stackryze | `*.indevs.in` 等 4 种 | 是 | 否 | 无 | 学生作业 / 测试站 |
| GitHub Pages | `*.github.io` | 是 | 否 | 无 | 静态博客 / 文档 |
| Cloudflare Pages | `*.pages.dev` | 是 | 否 | 无 | 面向全球的静态站 |
| Vercel | `*.vercel.app` | 是 | 否 | 无 | 前端 / 个人博客（非商业） |
| Netlify | `*.netlify.app` | 是 | 否 | 无 | 静态站 / 落地页 |
| Render | `*.onrender.com` | 是 | 否 | 无 | 全栈原型 / API |

> 结论先行：想要**真·二级域名**（能改 NS、接 Cloudflare）优先看 EU.org / is-a.dev / Stackryze；只想**白嫖建站子域名**直接 GitHub Pages / Cloudflare Pages / Vercel / Netlify / Render，全部免卡免审。

## EU.org

EU.org 是由欧洲非营利组织运营的老牌免费二级域名服务，自 1996 年运行至今，提供终身免费、无需续费的 `*.eu.org` 后缀（如 `blog.eu.org`）。服务完全免费，注册只需用户名和邮箱，无需信用卡、手机号或付费，原生支持 DNSSEC 与自定义 NS。申请入口为 https://nic.eu.org/ ：先 Register 注册账号，再进入 "Apply for a domain" 选择 Second-level domain，填入期望前缀并用英文简述用途后提交。主要坑在于人工审核时长极不稳定——历史案例常需数周至数月，但 2026 年部分教程称已提速到 3–7 个工作日，实际差异很大，需做好长时间等待的心理准备；且每 90 天需登录续期确认，否则可能被回收，域名所有权归 EU.org、不可交易。适用场景：对长期稳定性与合规性有要求的个人博客、自托管服务、Home Lab 等。

## is-a.dev

is-a.dev 是面向开发者的社区驱动开源服务，由 Cloudflare 的 Project Alexandria 项目赞助，提供免费的 `*.is-a.dev` 后缀（如 `yourname.is-a.dev`）。完全免费，无需信用卡或手机号；注册通过 GitHub 登录并 Fork 官方仓库 https://github.com/is-a-dev/register 提交 PR 完成。核心步骤：Fork 仓库后，在 `domains/` 目录新建 `<你的前缀>.json`，按要求填写 owner 与 records（支持 A/AAAA、CNAME、MX、TXT、URL 重定向等），提交 PR 由维护者审核，合并后 DNS 通常在几分钟内生效，典型审批 24–72 小时。主要坑：NS 记录受限（仅运行超 30 天且需充分理由的申请才放行），对 AI 生成的低质 PR 会直接关闭甚至封号，需遵守 ToS 且用途须为开发相关；已纳入公共后缀列表（PSL），被多数平台当作真实域名对待。适用场景：开发者作品集、技术博客、GitHub Pages/Vercel/Netlify 站点及个人 API。

## InfinityFree

InfinityFree 是 2013 年起运营、由 iFastNet 提供基础设施的免费主机服务，注册即赠送免费子域名 `*.rf.gd` 与 `*.epizy.com`（如 `yoursite.rf.gd`），无需绑卡、无强制广告。它本质是一套 PHP 主机（5GB 空间、无限带宽、PHP 8.3、400 个 MySQL 库、Softaculous 一键装 WordPress、免费 SSL），而非单纯的域名服务。申请地址 https://www.infinityfree.com/ ：用邮箱注册后点击 "Create Account"，在 Free Subdomain 中选定后缀并输入前缀即可秒级开通。主要坑：有 3 万 inode 上限与每日约 5 万访问的限流，高流量会被暂停；无 SSH、无邮件托管（仅支持外部邮箱）、不支持 NS 委派，后台含广告，需定期登录保持账号活跃。适用场景：个人博客、低流量 WordPress、学生练手与免费临时演示站，不适合高并发或商业生产环境。

## Stackryze Domains

Stackryze Domains 提供多个永久免费的二级域名后缀：`.indevs.in`、`.sryze.cc`、`.ryzedns.org`、`.nx.kg`。它完全免费，无需信用卡、无需手机号、无需实名或人工审核，仅靠邮箱或一键 GitHub 登录即可申请。新账号默认只能领 1 个 `.indevs.in` 域名；绑定 GitHub 并给 FreeDomains 项目点 Star 后，可解锁全部 4 个后缀各 1 个，合计 4 个。申请入口为 domain.stackryze.com，在控制台选前缀、加 DNS 记录（A/CNAME/MX）指向任意主机即可。需注意：官方虽主打"Forever free"，但每个域名有效期实为 1 年，到期前 60 天可在后台免费续期，并非一次注册永久占有。实测 `.nx.kg` 常因请求过多注册失败，建议优先选前三个；`.indevs.in` 与 `.sryze.cc` 可改 NS 接入 Cloudflare 享 CDN 与自动 HTTPS，`.ryzedns.org`、`.nx.kg` 暂不支持。适合学生作业、开源项目主页与个人测试站，不适合生产关键业务。

## GitHub Pages

GitHub Pages 为每位 GitHub 用户提供免费子域名 `*.github.io`（用户站为 `用户名.github.io`，项目站为 `用户名.github.io/仓库名`）。完全免费，只需免费 GitHub 账号，无需信用卡、无需手机号、无审核。申请要点：新建公开仓库（用户站仓库名须为 `用户名.github.io`），把静态文件（HTML/CSS/JS，或 Jekyll 输出）推到 main 分支，于仓库 Settings → Pages 选择分支并保存，约 1–2 分钟即上线，并自动获得 Let's Encrypt 签发的免费 HTTPS。可再绑定自定义域名：在 Pages 设置填域名，DNS 处加 CNAME（子域）或 4 条 A 记录（185.199.108–111.153，顶点域），传播后勾选 Enforce HTTPS 即可。主要坑：仅支持静态站点、无服务端运行；免费版仓库需公开；每月约 100 GB 软带宽上限；自定义域名建议先在账号设置里验证以防被他人接管。适合个人博客、文档与开源项目页。（本站就是 GitHub Pages 托管的。）

## Cloudflare Pages

Cloudflare Pages 为每个项目分配免费子域名 `*.pages.dev`（如 `项目名.pages.dev`），并自带全球 300+ 节点 CDN 与自动 HTTPS。完全免费，仅需免费 Cloudflare 账号（邮箱或 GitHub 授权登录），无需信用卡、无需手机号、无审核。申请要点：登录 dashboard.cloudflare.com，进入 Workers & Pages → Create → Pages → Connect to Git，授权 GitHub/GitLab 仓库并设定构建命令与输出目录，保存后自动部署，即获 `*.pages.dev` 地址；每次推送自动重建。可绑定自定义域名：在项目的 Custom domains 添加域名，若域名托管在 Cloudflare 则 DNS 与证书自动配好，外部 DNS 仅需加一条 CNAME 指向 `*.pages.dev`。主要坑：免费版每月构建上限 500 次、单文件 25 MB、站点文件总数 2 万；仅适合静态站与前端框架（需服务端请改用 Functions）；自定义域名不支持通配符。适合面向全球、追求无限带宽的静态站与前端应用。

## Vercel

Vercel 的免费档叫 Hobby，注册即用、自动分配 `*.vercel.app` 子域名（如 `your-project.vercel.app`），无需绑定信用卡。注册地址 vercel.com，支持 GitHub、GitLab、Bitbucket 或邮箱登录；连好 Git 仓库后每次 push 自动触发部署，也可装 Vercel CLI 用 `vercel` 命令发布。免费额度为每月 100GB 流量、100 万次边缘请求、100 万次函数调用、4 CPU 小时算力、每天最多 100 次部署，服务常驻、不会休眠。最大坑是 Hobby 计划严格限定"个人、非商业"用途——挂广告、接受捐赠、含支付链路或接单建站都需升级 Pro（$20/人/月），这是最易踩的雷；超额后功能暂停约 30 天而非自动扣费；且无内置数据库，需接外部存储。适合 Next.js/React 等前端项目、个人博客与作品集，不适合盈利型站点。

## Netlify

Netlify 免费档免信用卡（主流资料与官方注册流程均未要求绑卡，用 GitHub/Google 即可开通），部署后获得 `*.netlify.app` 子域名。注册地址 netlify.com，最大特点是"拖拽即部署"——把构建产物文件夹直接拖进控制台即可上线，也可连 Git 自动构建；支持 Netlify CLI（`netlify deploy --prod`）与 `netlify.toml` 配置。免费额度为每月 100GB 带宽、300 构建分钟、12.5 万次函数调用、100 次表单提交，站点常驻不休眠，且内置表单、分支预览、重定向与 A/B 测试，商业用途被允许。主要坑：函数单次执行上限 10 秒，无后台函数；月构建分钟偏紧，高频或慢构建易触顶；超额需升级 Personal（$9/月）或 Pro（$20/月）。适合静态站、文档、落地页及带轻量 API 的站点，对不会用 Git 的新手尤其友好。

## Render

Render 免费档无需信用卡（GitHub/GitLab/Google 直登即可），部署后分配 `*.onrender.com` 子域名，可部署真实后端 Web 服务、静态站及 PostgreSQL。注册地址 render.com，建服务时选 Static Site / Web Service 并在实例类型里挑 Free 即可，支持 Node.js、Python、Go、Ruby、Rust、Docker 等。免费额度含每月 750 实例小时、100GB 带宽、500 构建分钟、512MB 内存/0.1 CPU；静态站永久免费且常驻，商业用途允许，并可选部署区域。最大坑是免费 Web 服务空闲 15 分钟后自动休眠，下次访问需 30–60 秒冷启动（约 1 分钟），对公开访问体验不佳；免费 PostgreSQL 仅保留 30 天、之后 14 天宽限期即删数据；本地文件系统临时、重启即丢。适合 API/全栈原型、内部工具、Demo 与静态站，需常驻低延迟则升级 Basic（$6–7/月）关闭休眠。

---

> 带邀请链接、已单独详写的 3 篇：[DigitalPlat（qzz.io / dpdns.org）](/tech/digitalplat-free-domain/) · [kdns.fr](/tech/kdns-fr/) · [pp.ua](/tech/pp-ua/)。
