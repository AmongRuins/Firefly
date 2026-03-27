---
title: Giscus评论系统
published: 2026-03-09T18:25:28
description: 'giscus就是这样一个很好的评论区，它基于Github Discussion，无需自托管，无需你管理账号，仅需引入一个JS即可用！'
image: './images/covers/giscus.webp'
tags: [Giscus,评论]
category: 博客
draft: false 
lang: ''
---

## 配置 Giscus

> [你是否在寻找一个评论系统而又不想自托管？又饱受垃圾评论的叨扰？](https://2x.nz/posts/giscus-akismet/#%E9%85%8D%E7%BD%AEgiscus)、[无需服务器！为任何页面对接评论区！还有反垃圾！](https://www.bilibili.com/video/BV1GTtgziEp9/?spm_id_from=333.1391.0.0&vd_source=8abceb502969e7de8c2eb9bc66a1d6e3)

在 [Github](https://github.com/new) 中新建一个 **公开仓库**，然后在仓库**设置**中`单独`启用 `Discussion` 功能，最后看看 `Discussions` 可以访问不

![](./images/giscus/2025-08-04-12-16-36-image.webp)

前往 [giscus](https://giscus.app/zh-CN) 中配置你的仓库：

![](./images/giscus/2025-08-04-12-17-42-image.webp)

分类推荐选择 **公告（announcements）**

![](./images/giscus/2025-08-04-12-22-07-image.webp)

特性按需勾选

![](./images/giscus/2025-08-04-12-22-25-image.webp)

主题按喜好选择。更改后主题会立即呈现

![](./images/giscus/Snipaste_2026-03-09_18-46-52.png)

最后将这段 JS 复制，修改在文件路径 `/src/pages/posts/[...slug].astro` 中的 **Giscus 评论区**

![](./images/giscus/2025-08-04-12-23-41-image.webp)

## 配置 Akismet

> Akismet (Automattic Kismet) 是应用广泛的一个垃圾留言过滤系统，其作者是大名鼎鼎的 WordPress 创始人 Matt Mullenweg，Akismet 也是 WordPress 默认安装的插件，其使用非常广泛，设计目标便是帮助博客网站来过滤垃圾留言。

注册 [Akismet](akismet.com)，选择 Akismet Personal 订阅，将滑块拉到 `0$`，记录得到的 **Akismet API Key**。

![](./images/giscus/2025-08-04-12-27-58-image.webp)

在仓库中依次按 `.github/workflows/spam.yml` 来新建一个 `yml` 文件，其中内容复制该 [akismet-comment-check.yml](https://github.com/afoim/giscus-fuwari/blob/main/.github/workflows/akismet-comment-check.yml) 文件中的文本。

将这个 Github Action 部署到你的启用了 Github 的仓库中：

![](./images/giscus/Snipaste_2026-03-09_19-15-56.png)

配置 Secret：
- **AKISMET_API_KEY**：你的 `Akismet API Key`
- **GH_TOKEN**：前往 https://github.com/settings/tokens 创建一个具有 **repo write:discussion user** 权限的 Github 个人令牌

![](./images/giscus/2025-08-04-12-29-06-image.webp)

测试反垃圾是否有效，发送内容为 `viagra-test-123` 的评论。该评论一定会被当作垃圾评论。

查看 Github Action 是否做出删评行为：

![](./images/giscus/2025-08-04-12-30-37-image.webp)