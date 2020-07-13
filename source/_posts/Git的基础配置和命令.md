---
title: Git的基础配置与命令
date: 2020-07-09 20:38:57
tags:
---
## 一、SSH的配置
### 什么是SSH 
在我们的本地电脑和GitHub服务器之间，有一种保证本地电脑与我们GitHub之间的安全连接的暗号，它就是SSH（Secure Shell 安全协议外壳）。
ps：这个过程是在命令行中完成的
### 查看本地的SSH
可以使用 `cat ~/.ssh/id_rsa.pub` 这个命令来查看我们是否生成了SSH
### 生成本地的SSH
`ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`
### 获取本地SSH并绑定GitHub
使用`cat ~/.ssh/id_rsa.pub`获取本地SSH
在GitHub/Setting/SSH and GPG keys 新增SSH
#### SSH与电脑的系统环境相关，本地和虚拟机的SSH是不同的。

## 二、一些基础命令
### 全局变量设置
命令为
`git config --global user.name "叶冰"`
`git config --global user.email "hhdd576@126.com"`
这个命令只需进行一次 
### git clone 命令
git clone 的语法为`git clone 仓库地址` （仓库地址在仓库获取）
### 常见的Linux命令
`cd 文件`打开指定文件
`ls`查看文件
`ls -a`查看文件，包括隐藏文件
`touch`创建文件
`mkdir`创建文件夹
`rm -rf 文件`删除指定文件（**慎用**）
### git提交与git push
git提交一般分三步
1. git add -A
2. git commit -m "备注"
3. git push 
##### git push 情况比较特殊，分以下三种情况:
* 第一次提交到本分支`git push origin master`(origin 是远程仓库的默认名称，master是分支的默认名称)
* 第2--n次提交到本分支`git push`
* 提交到其他分支`git push origin b`
### git pull
在远程仓库与本地仓库彼此有一方是对方的真子集的情况下，代码正常。如果不是，会出现代码冲突，导致不能提交，在协同开发是会造成不必要的麻烦。
使用`git pull `可以抓取远程仓库的内容，替换本地的内容。

## 三、绑定本地仓库到GitHub
本地文件夹需初始为git仓库，在文件夹下使用`git init`命令将这个文件夹初始为git仓库。
查看绑定情况：`git remote -v`
绑定远程仓库：`git remote add origin 仓库地址`
移除错误绑定：`git remote remove origin`


