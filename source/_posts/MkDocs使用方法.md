---
title: MkDocs使用方法
toc: true
mathjax: true
tags:
  - mkdocs
  - markdown
  - blog
abbrlink: ce42b873
date: 2019-01-09 16:02:20
---

mkdocs是一款基于python markdown的项目文档工具,可以用来编写一个网站

<!--more-->
## 安装
#### 安装python3及pip
```shell
$ sudo apt install python3        #安装python
$ sudo apt install python3-pip    #安装pip
$ python3 --version               #检查python是否安装成功
$ pip3 --version                  #检查pip是否安装成功
```
#### 安装mkdocs
```shell
# pip3 install mkdocs             #注意以root权限安装
# mkdocs --version                #检查是否安装成功
```

## 使用
```shell
$ mkdocs new test                 #创建一个名为test的文件夹,存储代码
$ cd test
```
此时的目录结构
```test
test/
 ├── docs/                        #存放markdown文档
 │     └── index.md               #主页
 └── mkdocs.yml                   #配置文件
```
```shell
$ mkdocs serve                    #开启内建服务器
```
在浏览器中输入`127.0.0.1:8000`预览,终端键入`Ctrl+C`关闭服务器
```shell
$ mkdocs build                    #生成静态网页代码
```
这时已经生成了`site/`文件夹,可以将里面的内容部署到网站上了

## 配置文件
`site_name: `  __*必须存在*__,文档主标题名称
`site_favicon: `  图标,存放在`docs/`文件夹下
`theme: `  主题样式例如:
```yaml
theme: 
  name: 'material'                #使用material主题,需要pip安装mkdocs-material
  language: 'zh'                  #使用中文
  feature:
    tabs: true                    #使用上方tab栏（可改为false）
```
#### 目录结构
```yaml
nav:
  - 'Index': index.md
  - 'About': about.md
```
#### 扩展
执行`$ pip3 install pymdown-extensions`安装扩展包
```yaml
markdown_extensions:
  - admonition                    #支持注解
  - codehilite:                   #代码块高亮
      linenums: true              #代码块显示行号
  - pymdownx.tasklist             #支持任务列表
```

!!! warning "注意"
    一定要事先安装好扩展，否则不能出现预期效果

---
#### 参考
[MkDocs中文文档](https://markdown-docs-zh.readthedocs.io/zh_CN/latest/)
[MkDocs官方文档](https://www.mkdocs.org/)
[cyent的教程](https://cyent.github.io/markdown-with-mkdocs-material)