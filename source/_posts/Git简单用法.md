---
title: Git简单用法
toc: true
mathjax: true
date: 2019-01-09 16:18:10
tags: [git, github]
---

`Git`是一款版本控制软件，配合`GitHub`可以更好的控制代码

<!--more-->

### SSH Key
```
$ ssh-keygen -t rsa -C "975062472@qq.com"
$ cd ~/.ssh
$ cat id_rsa.pub
$ ssh -T git@github.com
```
### 创建仓库
```
$ mkdir study_cpp
$ cd study_cpp/
$ echo "# test" >> README.md
$ ls
README.md
$ git init
$ git add README.md
$ git commit -m "添加 README.md 文件"
$ git remote add origin git@github.com:Tony031218/study_cpp.git
$ git push -u origin master
```
### 克隆
```
$ git clone git@github.com:Tony031218/study_cpp
```
### 提取
```
$ git fetch origin
$ git merge origin/master
```
### 推送
```
$ git add <filename>
$ git commit -m "推送信息"
$ git push origin master
```
### 远程仓库
```
$ git remote add origin2 git@github.com:Tony031218/study_cpp.git
$ git remote -v
$ git remote rm origin2
$ git remote -v
```
### 分支
```
$ git checkout -b graph  //创建分支，并切换过去
$ git checkout master    //回到主分支
$ git push origin graph  //将分支推送到远程仓库
$ git pull               //将本地仓库更新
$ git diff graph master  //显示差别
```
### 克隆分支
```
$ git clone -b <branch_name> <repo_url>   //克隆单个分支
$ cd <repo>
$ git branch -a                           //查看所有分支
$ git checkout -b <branch_name> origin/<branch_name>  //关联分支
```