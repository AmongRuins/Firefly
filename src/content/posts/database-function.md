---
title: 数据库的安装和配置
published: 2026-04-07
description: '主要介绍数据库的作用以及MySQL的选择安装和配置'
image: './images/covers/mysql.webp'
tags: [软件测试]
category: 'MySQL数据库'
draft: false 
lang: ''
---

原生的文件系统在当前`互联网高并发`场景下，`检索效率极低`，`无法避免的数据一致性`，`发生故障很容易丢失数据`，导致研发成本变得极高。数据库则是`基于文件系统`来存储数据文件，并很好的解决文件系统的弊端。

当然对于比如图片、视频、音频等`大体积非结构化数据`依然适合用文件系统 / 对象存储存放，而这些文件的元数据（归属用户、上传时间、访问权限等），依然会存放在数据库中，二者配合使用，才是企业级系统的最优方案。

## 安装 MySQL

以 `8.0.45` 版本为例（其他版本的安装过程是类似的），下载 [MySQL 安装包（.msi）](https://dev.mysql.com/downloads/installer/)，双击开始安装。
![](./images/dbdownload/dbdowndold.webp)

勾选自定义 `custom`，然后点击 Next：
![](./images/dbdownload/dbpeiz1.webp)

在组件列表里逐层展开，勾选 `MySQL Server 8.0.45 - X64`，点击中的箭头，将他添加到右侧的窗口里：
![](./images/dbdownload/dbpeiz2.webp)

鼠标选中 `MySQL Server 8.0.41=5 - x64`，点击 `Advanced Options` ，将 MySQL 的安装路径改为其他盘（非系统盘）：
![](./images/dbdownload/dbpeiz3.webp)

最简单的路径修改方法，可以直接`将 C 改成 D`，然后点击 OK，在点击上图里的 Next。

点击 `Execute` 按钮，系统开始安装 `MySQL 8.0.45`，安装过程中会显示进度条，耐心等待安装完成：
![](./images/dbdownload/dbpeiz4.webp)

安装完成后(✅)，连续点击`两次` Next，进入到下面的界面(`默认即可`)，不要动它，点击 Next：
![](./images/dbdownload/dbpeiz5.webp)

官方推荐第一种，我们就用第一种，直接点击 Next：
> 注意，如果后面用到数据库图形化工具的话，例如 `navicat`，如果 navicat 版本太老，会产生数据库连接错误，这里建议选择第二个密码选项。

![](./images/dbdownload/dbpeiz6.webp)

在 `Password` 和 `Repeat Password` 输入框中，输入自定义的数据库密码，密码需为`字母、数字和特殊字符`，长度不少于 8 位，输入完成后，点击 Next：
![](./images/dbdownload/dbpeiz7.webp)

之后一直点击 `Next` 或 `Execute` 即可，都是默认安装配置，当看到该界面时，证明已安装完毕：
![](./images/dbdownload/dbpeiz8.webp)

## 配置 MySQL 的环境变量

右键点击`此电脑`，选择`属性`：
![](./images/dbdownload/huanjbianl1.webp)

在弹出的窗口中点击`高级系统设置`：
![](./images/dbdownload/huanjbianl2.webp)

在系统属性窗口中，点击`环境变量`按钮：
![](./images/dbdownload/huanjbianl3.webp)

在`系统变量`列表中，找到 `Path` 变量，点击`编辑`：
![](./images/dbdownload/huanjbianl4.webp)

点击`新建`，将 MySQL 的安装路径下的 `bin` 目录（例如：`D:\MySQL\MySQL Server 8.0\bin`）粘贴进去，点击`确定`保存设置：
![](./images/dbdownload/huanjbianl5.webp)

依次点击确定，`环境变量`就配置好了。

最后验证一下 MySQL 是否安装成功。按下键盘上的 `Win+R` 组合键（Windows 系统）或打开终端（Linux 系统），输入 `mysql -u root -p` 并回车。此时会提示`输入密码`，输入之前设置的数据库密码，然后回车：
![](./images/dbdownload/huanjbianl6.webp)

如果成功进入 MySQL 命令行界面，并显示 `Welcome to the MySQL monitor` 字样，说明 MySQL 8.0.45 安装成功。你可以开始使用 `CREATE DATABASE` 等命令创建数据库，进行`数据管理操作`了。

> 参考：[【2026最新】MySQL8下载安装全流程教程（附安装包+图文步骤）](https://developer.aliyun.com/article/1705141)

## 图形化界面下载

参考 [Navicat17安装教程（免费）,无需一键三连获取，评论区置顶免费获取](https://www.bilibili.com/video/BV1fSwvzXE1H/?spm_id_from=333.337.search-card.all.click&vd_source=8abceb502969e7de8c2eb9bc66a1d6e3)即可破解安装。
> 除了 Navicat，也推荐使用 `SQLyog` 作为工具使用。