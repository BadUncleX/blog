---
layout: post
title:  " Jekyll audio support "
date:   2019-05-08 16:30:11 +0800
categories: audio jekyll plugin
typora-copy-images-to: ../assets/images/2019-05
typora-root-url: ../../blog_alexcode
---
Jekyll blog audio support

## method 1 with iframe
update audio embedding.

## embed online audio player in markdown

> 不好使

```html
<iframe height="230" width="260" src="https://www.ximalaya.com/thirdparty/player/sound/player.html?id=156534134&type=red" frameborder=0 allowfullscreen></iframe>
```

> is that ok?

## embed mp3 file with jekyll plugin
> it's ok

<p>/assets/audio/napianhai.mp3</p>

## embed youtube video with jekyll plugin

```html
<p>https://youtu.be/iMvdG4guQf4</p>
```
<p>https://youtu.be/iMvdG4guQf4</p>

> cannot have blank space.

## embed youtube video just with iframe

<iframe width="560" height="315" src="https://www.youtube.com/embed/iMvdG4guQf4" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

> it's OK.