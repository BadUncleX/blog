---
layout: post
title:  " Jekyll blog configuration"
date:   2017-12-09 15:34:24 +0800
categories: jekyll
math: true
typora-copy-images-to: ../assets/images
typora-root-url: ../../alexcode
---
jekyll blog configuration


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
twitter_username: alexwanng
github_username:  alexwanng

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





## 继续语法高亮

updated: 2017-12-10 22:58:53



感觉在blog里放代码总有点分裂的感觉， 一份是本地的可运行代码， 一份是blog里的纯展示代码， 总感觉缺了点什么， 油水分离的感觉。 



最后觉得还是通过gist集成起来比较好。 

```html
<script src="https://gist.github.com/alexwanng/099cf7e6ee66e82199e2ca1f32b0fed0.js"></script>
```



另外， gist可以和其他代码一样git clone 到本地。 



另外， 如果有多个文件需要考虑排序的问题， 最简单的方式是在文件的前缀加上序号， 比如：

```html
1_ruby_quicksort.rb
2_keyboard_shortcuts.md
3_cheesecake_recipe.md
```



示例：

<script src="https://gist.github.com/alexwanng/a09de354ab75a12c9e87a977b8b85d62.js"></script>



<script src="https://gist.github.com/alexwanng/099cf7e6ee66e82199e2ca1f32b0fed0.js"></script>





