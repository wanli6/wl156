---
title: HexoBlog
date: 2020-07-10 12:16:22
tags:
---
## 一、什么是Hexo
hexo是一个快速、简洁、高效的博客框架，支持几条简单的命令发布博客。
## 二、如何安装Hexo
#### 准备工作
1. 需要电脑中有node.js  git
2. `npm install -g hexo-cli`运行命令安装Hexo
#### 创建博客文件夹
创建文件夹`hexo init blog`
#### 安装发布工具
`npm install hexo-deployer-git --save`
ps: 这个命令在上一步创建的文件夹中执行，如果创建了新的文件夹，则需要重新执行。
#### 修改配置
打开博客文件夹中的 `_config.yml` 这个文件
修改以下配置：
`theme: landscape`
`deploy:`
`   type: git`
`   repository: github仓库地址`
`   branch: master`
## 三、 Hexo的提交
hexo提交也分为三步
1. hexo clean 
2. hexo generate(hexo g)
3. hexo deploy(hexo d)

`hexo clean`  清除缓存让新发布的博客快速生效
`hexo g`  生成新的静态文件
`hexo d`  将生成的文件部署到博客上