---
layout: post
title:  " deadlock确定性死锁 "
date:   2018-03-28 15:34:24 +0800
categories: clojure promise deadlock
math: true
typora-copy-images-to: ../assets/images/2018-03
typora-root-url: ../../blog_alexcode
---
promise 实现确定性死锁， 即互相等待对方， 但是幸运的是， 这个死锁`每次都会发生`.

> 确定性的问题便于排查， 就怕间隙性神经病.





<script src="https://gist.github.com/foxlog/6c80eed40a4b3b9665a4a04dadfe5f93.js"></script>




