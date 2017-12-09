---
layout: post
title:  "Jekyll blog configuration"
date:  2017-12-09 16:06:17 +0800
categories: Jekyll
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

上传到github pages, 设置name:

```

```



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

```markdown
<h2>Categories (summary)</h2>
<ul>
{% assign categories_list = site.categories %}
  {% if categories_list.first[0] == null %}
    {% for category in categories_list %}
      <li><a href="#{{ category[0] }}">{{ category | capitalize }} ({{ site.tags[category].size }})</a></li>
    {% endfor %}
  {% else %}
    {% for category in categories_list %}
      <li><a href="#{{ category[0] }}">{{ category[0] | capitalize }} ({{ category[1].size }})</a></li>
    {% endfor %}
  {% endif %}
{% assign categories_list = nil %}
</ul>

{% for tag in site.categories %}
  <h3 id="{{ tag[0] }}">{{ tag[0] | capitalize }}</h3>
  <ul>
    {% assign pages_list = tag[1] %}
    {% for post in pages_list %}
      {% if post.title != null %}
      {% if group == null or group == post.group %}
      <li><a href="{{ site.url }}{{ post.url }}">{{ post.title }}<span class="entry-date"><time datetime="{{ post.date | date_to_xmlschema }}" itemprop="datePublished">{{ post.date | date: "%B %d, %Y" }}</time></span></a></li>
      {% endif %}
      {% endif %}
    {% endfor %}
    {% assign pages_list = nil %}
    {% assign group = nil %}
  </ul>
{% endfor %}
```





