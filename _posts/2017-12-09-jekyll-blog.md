---
layout: post
title:  " Jekyll blog configuration"
date:   2017-12-09 15:34:24 +0800
categories: jekyll
math: true
typora-copy-images-to: ../assets/images
typora-root-url: ../../alexcode
---
<h2>Table of contents</h2>
* TOC
{:toc}


## Jekyll 安装和运行

```bash
gem install jekyll
gem update jekyll

jekyll new myblog
cd myblog
jekyll server
bundle update jekyll
```



## Jekyll cname

上传到github pages, 设置name: www.alexcoding.me

## 核心配置

```yml
title: Alex ML blog
email: idea.wangtj at gmail.com
description: > # this means to ignore newlines until "baseurl:"
  Machine Learning, Algorithms, Reading and writing. 
baseurl: "" # the subpath of your site, e.g. /blog
url: "http://www.alexcoding.me" # the base hostname & protocol for your site, e.g. http://example.com
twitter_username: foxlog
github_username:  foxlog

# Build settings
markdown: kramdown
# theme: minima
plugins:
  - jekyll-feed


kramdown:
    input: GFM
    syntax_highlighter: rouge 


disqus:
    shortname: alexcoding
```



## 语法高亮

markdown指定为kramdown

## mathjax公式支持

```html
<script src='https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-MML-AM_CHTML'></script>

```

```markdown
{% if page.math != false %}
  {% include mathjax.html %}
{% endif %}

math: true

$$ x = {-b \pm \sqrt{b^2-4ac} \over 2a} $$
```



## 目录分类

 [参考这里](http://longqian.me/2017/02/09/github-jekyll-tag/)





