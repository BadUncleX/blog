---
layout: post
title:  " atom计时器with Executor "
date:   2018-03-28 15:34:24 +0800
categories: clojure atom executor threadpool
math: true
typora-copy-images-to: ../assets/images/2018-03
typora-root-url: ../../blog_alexcode
---
java的Executor做线程池submit.  可以将atom的计时器方法在executor里多次执行， 看看有没有问题。 



<script src="https://gist.github.com/alexwanng/c8bcebaa1f7ddadd59178fa2701ce098.js"></script>






