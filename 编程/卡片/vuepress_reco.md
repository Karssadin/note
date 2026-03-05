---
tags:
  - 计算机网络
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

# vuepress reco

需要下载node.js  
  
[vuepress reco快速指引](https://vuepress-theme-reco.recoluan.com/docs/guide/getting-started.html)

```Plain
npm install @vuepress-reco/theme-cli@1.0.7 -g
theme-cli init
npm install
```

站点结构如下

```Plain
    .vuepress/config.ts
    .vuepress/public
    /blogs
    README.md
```

所有的文章都在blog下面

`npm run build`build静态页面到./vuepress/dist中  
  
`npm run dev`启动本地端口进行访问静态页面

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
