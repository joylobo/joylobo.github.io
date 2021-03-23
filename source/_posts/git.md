---
title: Git
date: 2021-03-04 10:27:33
tags:
    - git
    - github
    - 版本管理
---

## 简介
Git是一个分布式版本控制系统(DVCS), 它可以跟踪文件的更改，并且可以恢复到任何特定版本的更改。Git项目主页: https://git-scm.com/。

## 入门篇 
### Git仓库的新建/克隆
```bash
git init                                         # 在当前目录新建一个Git仓库
git init [project-name]                          # 新建一个目录，将其初始化为Git仓库
git clone <remote_url>                           # 下载一个项目和它的整个代码历史
```
### 添加修改到暂存区
```bash
git add <filename>                               # 将对文件的修改添加到暂存区
git add <directory>                              # 将对文件夹的修改添加到暂存区
git add --all                                    # 将所有的修改添加到暂存区
git rm -cached target [-r]                       # 从暂存区删除文件
```
### 提交修改
```bash
git commit <filename> -m "提交日志"               # 提交指定文件
git commit -m "提交日志"                            
git commit -am "添加并提交"
git revert <commit id>                           # 创建一个新的提交，撤消错误提交中所做的更改
```
### 分支管理
```bash
git branch [-avv]                                # 查看当前分支
git branch <branch name>                         # 基于当前分支新建分支, 如:git branch dev 
git branch <branch name> <soure branch>          # 基于指定分支新建分支, 如:git branch dev master
git branch <branch name> <remote branch>         # 基于远程分支新建分支, 如:git branch dev origin/master
git branch <branch name> <commit id>             # 基于提交新建分支
git branch <branch name> <tag id>                # 基于tag新建分支
git branch -d <branch name>                      # 删除分支
git checkout <branch name>                       # 切换分支
git merge <merge target>                         # 合并分支, 当出现冲突时需解决后再提交
```
### Tag管理
```bash
git tag
git tag <tag name> <branch name>
git tag -d <tag name>
```
### 推送提交到远程
```bash
git remote -v                                    # 查看远程仓库
git remote add origin <url>                      # 添加远程仓库地址
git remote remove origin                         # 删除origin对应的远程仓库

git push
git push --set-upstream origin master
```
### 日志管理
```bash
git log                                          # 查看当前分支的提交日志
git log <branch name>                            # 查看指定分支的提交日志
git log --oneline                                # 单行显示提交日志
git log master..dev                              # 比较两个版本的区别
git log --pretty=format:'%h %s' --graph          # 以图表的方式显示提交合并网络
git show <branch name>
```
## 进阶篇
### [Git 内部原理 - Git 对象](https://git-scm.com/book/zh/v2/Git-%E5%86%85%E9%83%A8%E5%8E%9F%E7%90%86-Git-%E5%AF%B9%E8%B1%A1)
**Git存储对象**
Git是一个内容寻址文件系统，其核心部分是一个简单的键值对数据库(key-value data store)，你可以向数据库中插入任何内容，它会返回一个用户取回该键值的hash键。
```bash
git hash-object -w README.md                     # 元数据存储在.git/objects/目录
git cat-file -p [id]
```
**Git树对象**
它能解决文件名保存的问题，也允许我们将多个文件组织到一起。
**Git提交对象**
**Git引用**

## FAQ
**[关于 Git 的 20 个面试题](https://segmentfault.com/a/1190000019315509)**
### Git与svn的区别?
**存储方式不同**
Git采用元数据的方式存储，svn采用文件的方式（新版本已改为元数据）。
可以理解为svn存储的是整个文件，而git是将文件内容存储到数据库
**使用方式不同**
从本地把文件提交到远程，svn只需要commit，git则需要add、commit、push三个步骤
**管理模式不同**
git是分布式的，svn是集中式的。
