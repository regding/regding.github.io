---
layout: post
title: "部署在 Github Pages 的 Jekyll 站点使用 site.baseurl 时无法跳转到 site.baseurl 定义值的问题"
description: "基于 Jekyll 的站点使用 site.baseurl 参数，本地 Jekyll 环境以及 Coding.me 站点均正常跳转，但部署到 Github Pages 时无法正常跳转到 site.baseurl 定义值的问题解决方案"
date: 2017-07-22
tags: [Github, Jekyll]
categories: [Tech]
---

> 基于 Jekyll 搭建好个人静态 Blog 平台之后，好长时间都没空来写第一篇 Blog，今天写了第一篇 Blog 并发布到 Github Pages 之后，发现了一个问题，Google 之后，将这个问题和解决方案记录下来

### Contents
* [Problem][problem-contents-link]
* [Solution][solution-contents-link]
* [Reference][reference-contents-link]

---

[problem-contents-link]: #1-problem
[solution-contents-link]: #2-solution
[reference-contents-link]: #3-reference


### 1. Problem

描述一下问题。从我的 Blog 侧边栏导航点击 Home，或者点击每个页面上的 LOGO 头像（如下图），都应该是跳转到首页的。
在本地跑 Jekyll 以及发布到 Coding.me 站点都是正常的，但发布到 Github Pages 之后，发现点击侧边栏导航的 Home，或者页面上的 LOGO 头像，都无法跳转到首页。

![导航图][github-pages-site-baseurl-img-1]

打开浏览器的控制台，发现 LOGO 的 a 标签并没有 href 属性

![网页源码][github-pages-site-baseurl-img-2]

再对照 HTML 源码看看，由于源码已经修改过了，直接拿修改记录来看。下边是 Git diff 的比较，可以看到红色的那一行，之前 LOGO 区域的跳转链接是使用了 site.baseurl，而在 Jekyll 配置文件 `_config.yml` 中，定义了 site.baseurl 的值是 `/`，如图。

![LOGO源码][github-pages-site-baseurl-img-3]

![config.yml图][github-pages-site-baseurl-img-4]

可以看出，发布到 Github Pages 之后，site.baseurl 这个变量失效了！

---

[github-pages-site-baseurl-img-1]: {{ site.url }}/assets/posts_img/github-pages-site-baseurl/github-pages-site-baseurl-img-1.png
[github-pages-site-baseurl-img-2]: {{ site.url }}/assets/posts_img/github-pages-site-baseurl/github-pages-site-baseurl-img-2.png
[github-pages-site-baseurl-img-3]: {{ site.url }}/assets/posts_img/github-pages-site-baseurl/github-pages-site-baseurl-img-3.png
[github-pages-site-baseurl-img-4]: {{ site.url }}/assets/posts_img/github-pages-site-baseurl/github-pages-site-baseurl-img-4.png


### 2. Solution

问题已经知道了，这样就可以找到相应的解决方案。

由于我使用 site.baseurl 主要是为了跳转到网站首页，所以用相对路径 `/` 来代替 site.baseurl 这个网站域名的绝对路径就好了。修改如图中绿色的一行。

![LOGO源码][github-pages-site-baseurl-img-3]

侧边栏导航的修改方式也是一样，只要将 HTML 代码中的 site.baseurl 改为 `/` 就可以了。

对于我来说，这个问题算是解决了，但这个解决方案不一定适合其他人，所以再提供一些其他人的解决方案以及这个问题的描述，可以前往下列参考文档查看其中的内容。

---

[github-pages-site-baseurl-img-3]: {{ site.url }}/assets/posts_img/github-pages-site-baseurl/github-pages-site-baseurl-img-3.png


### 3. Reference

* [Jekyll官方对于该问题的描述及解决方案][reference-link-1]
* [Jekyll Github 上关于该问题的issues][reference-link-2]
* [stackoverflow 上关于该问题的问答][reference-link-3]

---

[reference-link-1]: https://jekyllrb.com/docs/github-pages/
[reference-link-2]: https://github.com/jekyll/jekyll/issues/332
[reference-link-3]: https://stackoverflow.com/questions/30209076/baseurl-behavior-differs-between-localhost-and-github-pages-in-jekyll


