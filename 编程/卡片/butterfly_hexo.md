---
tags: 
up: 
  - "[[搭建博客]]"
down: 
relation:
---
- [官方教程](https://butterfly.js.org/)
- [民间大神](https://www.fomal.cc/)
- [部署到githubPage上](https://zhuanlan.zhihu.com/p/60578464)

# 初始

```Plain
# 安装hexo
npm install -g hexo-cli
# 初始化hexo
hexo init blog-demo(项目名)
npm -i
# 安装主题
npm i hexo-theme-butterfly

# 创建对应的tags、categories
hexo new page tags
# 添加其中的tag

```

# css注入

```YAML
inject:
  head:
    - <link rel="stylesheet" href="/css/custom.css" media="defer" onload="this.media='all'">
    #动画标签anima的依赖
    - <link rel="stylesheet" href="<https://cdn.jsdelivr.net/gh/l-lin/font-awesome-animation/dist/font-awesome-animation.min.css>"  media="defer" onload="this.media='all'">
  bottom:
    # 阿里矢量图标,这串是我的图标库，你的链接会有所不同。
    - <script async src="//at.alicdn.com/t/font_2032782_ev6ytrh30f.js"></script>
    - <script defer src="<https://npm.elemecdn.com/jquery@latest/dist/jquery.min.js>"></script>
    - <script defer data-pjax src="/js/cat.js"></script>
    - <script async src="/js/title.js"></script>
```

# 网站资料

- 设置标题：
- 设置副标题，source有三种
- 描述以及作者在侧边栏出现

```YAML
title: 小孔碎碎念
subtitle:
  enable: true
  effect: true
  startDelay: 300 # time before typing starts in milliseconds
  typeSpeed: 150 # type speed in milliseconds
  backSpeed: 50 # backspacing speed in milliseconds
  # loop (循环打字)
  loop: true
  source: 3
  sub:
    - 笔记、杂记、随笔、心里话
description: '18岁 | 体育生 | 白袜 | 腹肌 | 185'
keywords:
author: 小孔
language: zh-CN
timezone: 'Asia/Shanghai'
```

# 主页

## 文章封面

- 设置文件封面，both在单栏时候生效

```YAML
cover:
  # 是否显示文章封面
  index_enable: true
  aside_enable: true
  archives_enable: true
  # 封面显示的位置
  # 三个值可配置 left , right , both
  position: both
  # 当没有设置cover时，默认的封面显示
  default_cover:
    - /pic/cover/1.jpg
    - /pic/cover/2.jpg
    - /pic/cover/3.jpg
    - /pic/cover/4.jpg
```

## 主页文章节选

- 设置主页文章摘选是选择文章中的discrip还是前面几个字

```YAML
index_post_content:
  method: 2
  length: 500 # if you set method to 2 or 3, the length need to config
```

# 404页面

- 在config.yml中配置404页面

```YAML
error_404:
  enable: true
  subtitle: "页面没有找到"
  background:
    - /pic/cover/4.jpg
```

# 所有页面

## 导航栏

```YAML
nav:
  logo: /pic/logo.svg
  display_title: true
  fixed: false # fixed navigation bar
```

### 本地搜索

```YAML
search:
  path: search.xml
  field: post
  content: true

local_search:
  enable: true
```

## 页面底部

```YAML
footer_bg: false
footer:
  owner:
    enable: true
    since: 2021	# 站点起始时间
  custom_text:	任何选择都有遗憾<p><a href="<https://beian.miit.gov.cn/>"><span>冀ICP备2023021918号</span></a>
  copyright: false # Copyright of theme and framework
```

## 菜单

menu:  
主页: / || fas fa-home  
归档: /archives/ || fas fa-archive  
标签: /tags/ || fas fa-tags  
分类: /categories/ || fas fa-folder-open  

## 侧边栏

```YAML
aside:
  enable: true
  hide: false
  button: true
  mobile: true # display on mobile
  position: left # left or right
  display:
    archive: true
    tag: true
    category: true
  card_author:
    enable: true
    description:
    button:
      enable: false
  card_announcement:
    enable: true
    content: 开心每一天！
  card_recent_post:
    enable: true
    limit: 5 # if set 0 will show all
    sort: date # date or updated
    sort_order: # Don't modify the setting unless you know how it works
  card_categories:
    enable: true
    limit: 8 # if set 0 will show all
    expand: none # none/true/false
    sort_order: # Don't modify the setting unless you know how it works
  card_tags:
    enable: true
    limit: 40 # if set 0 will show all
    color: false
    orderby: random # Order of tags, random/name/length
    order: 1 # Sort of order. 1, asc for ascending; -1, desc for descending
    sort_order: # Don't modify the setting unless you know how it works
  card_archives:
    enable: true
    type: monthly # yearly or monthly
    format: MMMM YYYY # eg: YYYY年MM月
    order: -1 # Sort of order. 1, asc for ascending; -1, desc for descending
    limit: 8 # if set 0 will show all
    sort_order: # Don't modify the setting unless you know how it works
  card_webinfo:
    enable: true
    post_count: true
    last_push_date: true
    sort_order: # Don't modify the setting unless you know how it works
  card_post_series:
    enable: true
    orderBy: 'date' # Order by title or date
    order: -1 # Sort of order. 1, asc for ascending; -1, desc for descending
```

### 访问人数、运行时间、

```YAML
busuanzi:
  site_uv: true  # 本站总访客数
  site_pv: true  # 本站总访问量
  page_pv: true  # 本文总阅读量
runtimeshow:
  enable: true
  publish_date: 8/9/2022 00:00:00
```

### 头像

```YAML
avatar:
  img: /pic/logo.svg
  effect: false # true则会一直转圈
```

## 右下功能键

### 简繁转换

```YAML
translate:
  enable: true
  # 默认按钮显示文字(网站是简体，应设置为'default: 繁')
  default: 简
  #网站默认语言，1: 繁体中文, 2: 简体中文
  defaultEncoding: 1
  #延迟时间,若不在前, 要设定延迟翻译时间, 如100表示100ms,默认为0
  translateDelay: 0
  #当文字是简体时，按钮显示的文字
  msgToTraditionalChinese: "繁"
  #当文字是繁体时，按钮显示的文字
  msgToSimplifiedChinese: "简"
```

### 夜间模式

```YAML
darkmode:
  enable: true
  button: true
  autoChangeMode: false
```

### 阅读模式按钮

```YAML
readmode: true
```

### 滚动状态百分比

```YAML
rightside_scroll_percent: true
```

## 顶部图

- 设置为透明，可以看到背景图片

```YAML
index_img: transparent
default_top_img: transparent
archive_img: transparent
tag_img: transparent
category_img: transparent
category_per_img: transparent
tag_per_img: transparent
```

# 文章页面

## 代码

```YAML
highlight:
  enable: true
  line_number: false
  auto_detect: false
  tab_replace: ''
  wrap: true
  hljs: false
highlight_theme: mac
highlight_shrink: true #代码框不展开，需点击 '>' 打开
highlight_copy: true
```

## 文章内容复制相关

```YAML
copy:
  enable: true	# 是否开启网站复制权限
  copyright:	# 复制的内容后面加上版权信息
    enable: true	# 是否开启复制版权信息添加
    limit_count: 50	# 字数限制，当复制文字大于这个字数限制时，将在复制的内容后面加上版权信息
```

## 文章版权和协议许可

- 文章底部的版权许可

```YAML
post_copyright:
  enable: true
  decode: false
  author_href:
  license: CC BY-NC-SA 4.0
  license_url: <https://creativecommons.org/licenses/by-nc-sa/4.0/>
```

## 文章目录TOC

```YAML
toc:
  post: true
  page: true
  number: true
  expand: false
  style_simple: false # for post
  scroll_percent: true
```

## 文章元信息显示

- 文章中详细信息展示多少

```YAML
post_meta:
  page:
    date_type: both # created or updated or both 主页文章日期是创建日或者更新日或都显示
    date_format: relative # date/relative 显示日期还是相对日期
    categories: true # true or false 主页是否显示分类
    tags: true # true or false 主页是否显示标签
    label: true # true or false 显示描述性文字
  post:
    date_type: both # created or updated or both 文章页日期是创建日或者更新日或都显示
    date_format: data # date/relative 显示日期还是相对日期
    categories: true # true or false 文章页是否显示分类
    tags: true # true or false 文章页是否显示标签
    label: true # true or false 显示描述性文字
```

## 相关文章

```YAML
related_post:
  enable: true
  limit: 6 # 显示推荐文章数目
  date_type: created # or created or updated 文章日期显示创建日或者更新日
```

## 文章分页提醒

```YAML
# value: 1 || 2 || false	# false:为关闭分页按钮；1:下一篇显示的是旧文章；2:下一篇显示的是新文章
# 1: The 'next post' will link to old post
# 2: The 'next post' will link to new post
# false: disable pagination
post_pagination: 1
```

## 是否启用文章加密

```YAML
encrypt:
  enable: true
```

## 字数统计

```YAML
wordcount:
  enable: true
  post_wordcount: true
  min2read: true
  total_wordcount: true
```

butterfly_article_double_row:  
enable: true  

# 美化

## 网站背景

```YAML
background: url(<https://bing.img.run/rand_m.php>)
```

## 动态彩带

```YAML
canvas_fluttering_ribbon:
  enable: true
  mobile: true # false 手机端不显示 true 手机端显示
```

## 鼠标点击

```YAML
click_heart:
  enable: true
  mobile: true
```

## 加载动画 Loading Animation

```YAML
preloader: true
```

## 美化页面显

```YAML
beautify:
  enable: true
  field: post # site/post
  # title-prefix-icon: '\\f0c1' 原内容
  title-prefix-icon: '\\f863'
  title-prefix-icon-color: "\#F47466"
```