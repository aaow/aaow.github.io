---
title: Hexo博客和Fluid主题
categories: Hexo博客
tags:
- hexo
abbrlink: 187bd52a
date: 2023-09-18 00:08:35
banner_img: 
index_img: https://raw.githubusercontent.com/aaow/blog_img/main/2023/202311060048328.jpg

---

# Hexo博客记录

> nvm 1.1.11 #nodejs版本管理
> nodejs v20.6.1
> npm 10.1.0 #包管理
> hexo 6.3.0#博客管理
> fluid  #博客主题

## 安装Hexo 

文档地址: https://hexo.io/zh-cn/docs/

新建一个文件夹，安装hexo-cli: 

```bash
$ npm install -g hexo-cli
```

进入文件夹，初始化: 

```bash
$ hexo init <folder>
$ npm install
```

生成目录:

```
.
├── _config.yml #主配置
├── package.json
├── scaffolds
├── source #资源
|   ├── _drafts
|   └── _posts
└── themes #主题
```

## 安装主题fluid

下载主题到themes文件夹:

```bash
$ git clone https://github.com/fluid-dev/hexo-theme-fluid.git themes\fluid
```

把主题配置config复制出来:

```bash
$ cp themes\fluid\_config,yml _config.fluid.yml
```

指定主配置 `_config.yml`: 

```yml
theme: fluid
```

## 一键部署

安装hexo-deployer-git

```bashdeploy:
$ npm install hexo-deployer-git --save
```

在`_config.yml`中添加配置: 

```yml
deploy:
  type: git
  repo: https://github.com/<username>/<project>
  branch: gh-pages
```

部署命令:

```bash
$ hexo clean && hexo g && hexo d
```

{% note success %}

npm有依赖问题，使用yarn 安装:

```bash
$ npm install -g yarn
$ yarn #默认install
```

{% endnote %}



## 本地部署

```bash
$ hexo s
```



