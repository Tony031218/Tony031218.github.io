---
title: Hexo搭建博客
toc: true
mathjax: true
date: 2019-03-12 12:50:18
tags: [hexo, blog, markdown]
---

由于`mkdocs`上有很多不足，例如没有标签，分类，评论，计数等等，故转至使用`Hexo`搭建博客，以下是我折腾的过程

<!--more-->

## 安装Hexo
我是用的是`Ubuntu16.04`系统，其他系统安装方法可到官网查询
在[nodejs官网](https://nodejs.org)下载`node.js`，并解压
添加环境变量
`echo "export PATH=$PATH:/home/tony/node-v8.11.4-linux-x64/bin" >> ~/.zshrc`（如果使用的是bash，将最后一句改为`~/.bashrc`）
`source ~/.zshrc`应用更改

下载hexo
`npm install -g hexo-cli`

## 搭建博客
```shell
$ mkdir blog
$ cd blog
$ hexo init
```
使用`hexo s`启动服务,在浏览器中输入`localhost:4000`便可看到预览网页

### 部署到GitPages
创建`<username>.github.io`存储库
更改`_config.yml`文件中这一部分
```yml
deploy:
  type: git
  repository: git@github.com:<username>/<username>.github.io.git
  branch: master
```
使用`hexo d -g`发布

## 主题
我当前使用的是`maupassant`主题
详细配置见[官方中文文档](https://www.haomwei.com/technology/maupassant-hexo.html)

## 参考
[Hexo官网](https://hexo.io)
[zzq浅谈用Hexo+GitHub搭建自己的blog](https://afar5277.blog.luogu.org/post-zzq-hexoblog)
[Hexo博客搭建说明书（指北书）](https://www.luogu.org/blog/0Umaru0/hexo-bo-ke-da-jian-shuo-ming-shu-zhi-bei-shu-post)
[从零搭建 Hexo + Github 博客](https://www.luogu.org/blog/Venus/build-hexo-github-blog)