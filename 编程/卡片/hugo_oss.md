---
tags: 
up: 
  - "[[搭建博客]]"
down: 
relation:
---
# 购买域名和oss服务

## 域名

- 之前已经购买过了，但是没有进行备案，之后的oss服务需要选择香港的，不然无法进行绑定未备案的域名

## oss服务

- 阿里云中找到`对象存储OSS`
- 点击`创建bucket`，`区域`选择香港，`存储类型`因为是静态网页，选择`低频访问`就行，`读写权限`选择公共读，`服务端加密、同城冗余存储、实时日志查询`都不开通
- 基础设置中搜`静态页面`，设置`默认首页`为index.html，`子目录`首页开通，`404规则`Redirect，`默认404页`为404.html，`错误文件响应码`为404
- 基础设置中搜`域名管理`，进行域名的绑定，需要将`20001004.xzy、www.20001004.xzy`都进行绑定
- 绑定完成之后oss的设置就完成了，使用OSS browser进行文件的上传比较方便，需要使用阿里云的AK进行验证

# hugo

[hugo快速指引](https://www.gohugo.org/doc/overview/quickstart/)

[hugoGithub进行下载](https://github.com/gohugoio/hugo/releases)  
下载完之后对exe文件进行配置环境变量  

`hugo new site path-to-site`

`cd path-to-site`

站点目录结构如下

```Plain
    archetypes/
    doc/content/
    data/
    layouts/
    static/
    config.toml
```

所有的文章都在post下面  
  
`hugo new post/first.md`[hugo主题链接](https://themes.gohugo.io/)

下载主题，放到themes库中

`hugo -D` build静态页面到public中  
  
`hugo -D server`启动本地端口进行访问静态页面

## 编写md

- 在post下面编写即可
- markdown表头添加一些tag来进行分类，时间管理

```Markdown

---
title: "博客名字"
date: 时间
categories:
    - 类别
tags:
    - 标签
author: 作者
---

```