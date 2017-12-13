---
layout: post
title:  "awesome clojure"
date:   2017-12-13 08:57:57 +0800
categories: clojure awesome
math: true
typora-copy-images-to: ../assets/images/2017-12
typora-root-url: ../../alexcode
---
<h2>Table of contents</h2>
* TOC
{:toc}

[参考](https://github.com/razum2um/awesome-clojure) 

> 选取近期会用到的， 以及自己整理的部分内容， 不定期更新，方便检索



## awesome products in Clojure

- [LightTable](http://lighttable.com/) (大牌代码编辑器) (9955 ★)
- [Nightcode](https://sekao.net/nightcode/)(代码编辑器) (1178 ★)



## web framework

- [Compojure](https://github.com/weavejester/compojure) (3305 ★) (A concise routing library for Ring/Clojure)

- [Luminus](http://www.luminusweb.net/) (Luminus is a Clojure micro-framework based on a set of lightweight libraries. It aims to provide a robust, scalable, and easy to use platform. With Luminus you can focus on developing your app the way you want without any distractions.)

- [Duct](https://github.com/weavejester/duct) (612 ★)Server-side application framework for Clojure. Duct is a highly modular framework for building server-side applications in Clojure using data-driven architecture.

  It is similar in scope to [Arachne](http://arachne-framework.org/), and is based on [Integrant](https://github.com/weavejester/integrant). Duct builds applications around an immutable configuration that acts as a structural blueprint. The configuration can be manipulated and queried to produce sophisticated behavior.

- [Pedestal](https://github.com/pedestal/pedestal)(1838 ★) Pedestal is a set of libraries written in Clojure that aims to bring both the language and its principles (Simplicity, Power and Focus) to server-side development.

  **Pedestal requires Java 1.8+ and Servlet 3.1**

- [yada](https://github.com/juxt/yada) (500 ★)yada is a web library for Clojure, designed to support the creation of production services via HTTP.

  It has the following features:

  - Standards-based, comprehensive HTTP coverage (content negotiation, conditional requests, etc.)
  - Parameter validation and coercion, automatic Swagger support
  - Rich extensibility (methods, mime-types, security and more)
  - Asynchronous, efficient interceptor-chain design built on [manifold](https://github.com/ztellman/manifold)
  - Excellent performance, suitable for heavy production workloads

  ​

## Build Automation and Package management

- [Leiningen](https://github.com/technomancy/leiningen) (5719 ★) 老牌build工具
- [Boot](https://github.com/boot-clj/boot) (1379 ★) 新生代



## http

- [clj-http](https://github.com/dakrone/clj-http) (1141 ★) An idiomatic clojure http client wrapping the apache client. 
- [http-kit](http://www.http-kit.org/) （1695 ★ by [shenfeng](https://github.com/shenfeng)）
- [ring](https://github.com/ring-clojure/ring)（2414 ★） Clojure HTTP server abstraction。 Ring is a Clojure web applications library inspired by Python's WSGI and Ruby's Rack. By abstracting the details of HTTP into a simple, unified API, Ring allows web applications to be constructed of modular components that can be shared among a variety of applications, web servers, and web frameworks.



## ORM and SQL generation

- [Korma](http://sqlkorma.com/) （1313 ★）



## RESTful API

- [Liberator](http://clojure-liberator.github.io/liberator/) （1121 ★）
- [yada](https://github.com/juxt/yada) （500 ★）（A powerful Clojure web library, full HTTP, full async）



## Pattern Matching

- [core.match](https://github.com/clojure/core.match) （804 ★）
- [defun](https://github.com/killme2008/defun) （343 ★ by [dennis](https://github.com/killme2008)）



## Machine Learning

- [cortex](https://github.com/thinktopic/cortex) （913 ★ ）Neural networks, regression and feature learning in Clojure.
- [Deeplearning4j](https://github.com/deeplearning4j/deeplearning4j) （7862 ★ ） Deep Learning for Java, Scala & Clojure on Hadoop & Spark With GPUs



## Guides

- [The Clojure Style Guide](https://github.com/bbatsov/clojure-style-guide)
- [Clojure Distilled](http://yogthos.github.io/ClojureDistilled.html)
- [clojure-cookbook](https://github.com/clojure-cookbook/clojure-cookbook)
- [A Brief Beginner's Guide To Clojure](http://www.unexpected-vortices.com/clojure/brief-beginners-guide/index.html)
- [Clojure for the Brave and True](http://www.braveclojure.com/)
- [Clojure from the ground up](https://aphyr.com/tags/Clojure-from-the-ground-up)
- [Clojure by Example](https://kimh.github.io/clojure-by-example/)



## web site

- [Clojure](http://clojure.org/)
- [Clojure Slack](http://clojurians.net/)
- [clojuredocs](http://clojuredocs.org/)
- [crossclj](https://crossclj.info/)
- [clojure-doc](http://clojure-doc.org/)
- [Grimoire](http://conj.io/)
- [The Clojure Toolbox](http://www.clojure-toolbox.com/)
- [InstaREPL Online](http://web.clojurerepl.com/)
- [ZEEF/Clojure](https://clojure.zeef.com/vlad.bokov)
- [Try Clojure](http://www.tryclj.com/)



## books

[from here](https://clojure.org/community/books)

*Listed in order of descending release date of newest edition.*



Updated: release date after 2015. 

| [![ClojureScript Unraveled](http://ecx.images-amazon.com/images/I/41k50H6VpaL._SL160.jpg)](http://a.co/cDfN4n4) | [Quick Clojure: Effective Functional Programming](http://a.co/cDfN4n4)by Mark McDonnellAug 23, 2017 |
| ---------------------------------------- | ---------------------------------------- |
| [![Web Development with Clojure](http://ecx.images-amazon.com/images/I/518xLvhHZ1L._SL160.jpg)](http://a.co/c2gI4l2) | [Web Development with Clojure](http://a.co/c2gI4l2)by Dmitri SotnikovAug 23, 2016 |
| [![ClojureScript Unraveled](https://s3.amazonaws.com/titlepages.leanpub.com/clojurescript-unraveled/small)](https://leanpub.com/clojurescript-unraveled) | [ClojureScript Unraveled](https://leanpub.com/clojurescript-unraveled)by Andrey Antukh, Alejandro GómezJul 25, 2016 |
| [![Learning ClojureScript](http://ecx.images-amazon.com/images/I/51EwRiXh4ZL._SL160.jpg)](http://a.co/2X3MJn2) | [Learning ClojureScript](http://a.co/2X3MJn2)by W. David Jarvis, Rafik Naccache, Allen RohnerJun 30, 2016 |
| [![Professional Clojure](http://ecx.images-amazon.com/images/I/51iq-PKIZ8L._SL160.jpg)](http://a.co/bSHZ7X3) | [Professional Clojure](http://a.co/bSHZ7X3)by Jeremy Anderson, Michael Gaare, Justin Holguín, Nick Bailey, and Timothy PratleyJun 7, 2016 |
| [![Mastering Clojure](http://ecx.images-amazon.com/images/I/61TJZjnjO0L._SL160.jpg)](http://a.co/bTLhJ2d) | [Mastering Clojure](http://a.co/bTLhJ2d)by Akhil WaliMar 28, 2016 |
| [![Clojure for Java Developers](http://ecx.images-amazon.com/images/I/61p47dd81cL._SL160.jpg)](http://a.co/029aVrm) | [Clojure for Java Developers](http://a.co/029aVrm)by Eduardo DiazFeb 23, 2016 |
| [![Clojure for Finance](http://ecx.images-amazon.com/images/I/51ofF2ckdkL._SL160.jpg)](http://a.co/fbHnhEM) | [Clojure for Finance](http://a.co/fbHnhEM)by Timothy WashingtonJan 11, 2016 |
| [![Clojure In Action](http://ecx.images-amazon.com/images/I/51QWOEjmtIL._SL160.jpg)](http://a.co/a4hDbTn) | [Clojure In Action](http://a.co/a4hDbTn)by Amit RathoreJan 1, 2016 |
| [![Clojure for the Brave and True](http://ecx.images-amazon.com/images/I/6112vbQYDLL._SL160.jpg)](http://a.co/bsviqV7) | [Clojure for the Brave and True](http://a.co/bsviqV7)by Daniel HigginbothamOct 23, 2015 |
| [![Clojure Recipes](http://ecx.images-amazon.com/images/I/51aMgNS%2BK7L._SL160.jpg)](http://a.co/clSHVQi) | [Clojure Recipes](http://a.co/clSHVQi)by Julian GambleOct 23, 2015 |
| [![Clojure Applied](http://ecx.images-amazon.com/images/I/41iH5aTHB3L._SL160.jpg)](http://a.co/1HL2XPF) | [Clojure Applied: From Practice to Practitioner](http://a.co/1HL2XPF)by Ben Vandgrift, Alex MillerSept 6, 2015 |
| [![Clojure for Data Science](http://ecx.images-amazon.com/images/I/51ki-47i6bL._SL160.jpg)](http://a.co/idtKjhS) | [Clojure for Data Science](http://a.co/idtKjhS)by Henry GarnerSept 3, 2015 |
| [![Clojure High Performance Programming](http://ecx.images-amazon.com/images/I/51Nym1wJXVL._SL160.jpg)](http://a.co/7adcmsl) | [Clojure High Performance Programming](http://a.co/7adcmsl)by Shantanu KumarSept 1, 2015 |
| [![Clojure Data Structures and Algorithms](http://ecx.images-amazon.com/images/I/515vh5czqnL._SL160.jpg)](http://a.co/g7JAFAS) | [Clojure Data Structures and Algorithms](http://a.co/g7JAFAS)by Rafik NaccacheAug 19, 2015 |
| [![Living Clojure](http://ecx.images-amazon.com/images/I/5122uV93jfL._SL160.jpg)](http://a.co/1m2Zt4p) | [Living Clojure](http://a.co/1m2Zt4p)by Carin MeierApr 30, 2015 |
| [![Clojure Reactive Programming](http://ecx.images-amazon.com/images/I/51l1oGz9N7L._SL160.jpg)](http://a.co/fhyaFka) | [Clojure Reactive Programming](http://a.co/fhyaFka)by Leonardo BorgesMar 24, 2015 |
| [![Clojure Web Development Essentials](http://ecx.images-amazon.com/images/I/51XnilmUaIL._SL160.jpg)](http://a.co/2FlRxd5) | [Clojure Web Development Essentials](http://a.co/2FlRxd5)by Ryan BaldwinFeb 16, 2015 |
| [![Clojure Data Analysis Cookbook](http://ecx.images-amazon.com/images/I/51-B3kElSiL._SL160.jpg)](http://a.co/gIwPEkt) | [Clojure Data Analysis Cookbook](http://a.co/gIwPEkt)by Eric RochesterJan 22, 2015 |