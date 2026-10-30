---
title: 我的博客是怎么搭起来的
published: 2026-10-30
description: 从零开始，用 Astro + GitHub Pages 搭建了这个博客。记录一下完整的过程和踩过的坑。
tags: [建站, 随笔]
category: 随笔
draft: false
lang: zh_CN
---

# 我的博客是怎么搭起来的

这个博客今天正式上线了。趁着新鲜劲儿还热乎，把搭建过程记录下来。

## 为什么要写博客

想了想，主要是两个原因：

1. **留痕**。平时折腾工具、解决问题，很多过程过段时间就忘了，写下来以后自己还能翻
2. **练手**。有个自己的小站，遇到想记录的东西就顺手发一篇

## 技术选型

我不懂后端，所以选型的核心要求就是：**不碰服务器、不碰数据库**。

最终方案：

- 框架：[Astro](https://astro.build)
- 主题：[Fuwari](https://github.com/saicaca/fuwari)（一个很漂亮的 Astro 博客模板）
- 托管：GitHub Pages（免费）
- 写作：纯 Markdown

为什么选静态博客？因为所有页面都是**提前编译好的 HTML 文件**，访客拿到的就是现成的文件——没有服务器要维护，没有数据库会被黑，不用花一分钱。

## 搭建过程

整个过程其实就三步。

### 第一步：本地跑起来

环境只需要 Node.js 和 pnpm：

```bash
git clone https://github.com/saicaca/fuwari.git my-blog
cd my-blog
pnpm install
pnpm dev
```

打开 `localhost:4321`，博客就在本地跑起来了。改配置、写文章，保存后浏览器实时刷新。

### 第二步：改成自己的

主要是改 `src/config.ts`：博客名、头像、简介、社交链接，都在这一个文件里。文章则是往 `src/content/posts/` 目录里扔 Markdown 文件。

### 第三步：推上 GitHub

在 GitHub 建一个名为 `用户名.github.io` 的仓库（这个命名规则会自动获得一个免费网址），然后：

```bash
git push
```

GitHub Actions 会自动完成剩下的事：装依赖、把整个网站编译成静态文件、发布到 GitHub Pages 的 CDN 上。一分钟后，博客就上线了。

## 踩过的坑

::note
记录两个真实的报错，万一你也在搭 Fuwari，能少走弯路。
::

**坑一：pnpm 版本声明冲突**

GitHub Actions 部署时报错：

```
Error: Multiple versions of pnpm specified
```

原因是工作流文件里写了 `version: 9`，而 `package.json` 里又声明了 `pnpm@9.14.4`。**版本只能声明一次**——删掉工作流里的那个就好。

**坑二：构建报 `The 'link' class does not exist`**

Fuwari 模板自带的 CSS 在一个文件里 `@apply` 了另一个文件定义的自定义类，生产构建时解析不到。把那个类的样式展开内联到当前文件里，问题消失。

## 以后写什么

大概会是这些方向：

- iOS 开发笔记
- 折腾环境和工具的心得
- 一些随笔

以上。博客开通，欢迎常来。

::github{repo="saicaca/fuwari"}
