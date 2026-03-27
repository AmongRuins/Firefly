---
title: Firefly 快速开始
published: 2026-03-23
description: '快速配置且部署 Firefly'
pinned: true
image: './images/covers/firefly.webp'
tags: [Firefly主题]
category: '博客'
draft: false 
lang: 'zh-CN'
---

## 前置环境准备

Firefly 主题依赖 Node.js 和包管理工具，先完成环境搭建，避免后续部署报错。

下载安装 [Node.js](https://nodejs.org/zh-cn)，完成后通过终端安装 pnpm，运行如下命令：
```bash
npm install -g pnpm
```

---

## 主题下载与本地启动

[Firefly](https://github.com/CuteLeaf/Firefly) 主题博客文件需要在 `Github` 中下载，但打开或下载都要`等很久`，原因在于：
- **DNS污染**：就像快递员`找不到`你家地址
- **服务器物理距离**：GitHub 服务器主要在美国，`物理延迟`200ms起步
- **带宽限制**：高峰期就像早高峰地铁，百万开发者`挤一条线路`
- **特殊网络环境**：某些地区需要特别处理（`魔法`）

这里最简单快速的方法是将 `Gitee` 作为中转站，在 [这里](https://gitee.com/projects/import/url)  导入你 Github 仓库链接（`Fork Firefly生成`），之后就可以将文件通过 `Gitee` 下载到本地。

下载进本地的博客仓库是不能上传到 `Github` 中的，原因在于你的上传地址被改成 `Gitee` 的了。

打开终端或命令提示符，运行以下命令`修改上传地址`：
```bash
git remote set-url origin https://github.com/AmongRuins/Firefly
```

安装项目依赖，执行如下命令：
```bash
pnpm install
```

启动开发服务器，执行如下命令：
```bash
pnpm dev
```

启动成功后，浏览器访问 `http://localhost:4321` 即可查看默认博客界面，Ctrl+C 可关闭服务器。

---

## 博客部署上线

[Vercel](https://vercel.com/) 是部署 Astro 项目最简单的平台之一。具体部署步骤如下：
1. 将 Firefly 项目上传至自己的 GitHub 仓库
2. 登录 Vercel，点击 `New Project`
3. 导入 GitHub 仓库，无需修改配置，直接 `Deploy`
4. 部署成功后，Vercel 会自动分配`域名`，但国内需要魔法才能访问

当然也可绑定自定义域名，这边可以在 [GNAME](https://gname.vip/user#/domain_info/ym=amongruins.eu.cc) 中仅通过邮箱`白嫖三年`的`未备案`的 `.eu.cc` 域名。