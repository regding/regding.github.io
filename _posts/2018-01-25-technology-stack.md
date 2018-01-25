---
layout: post
title: "侃侃技术栈(Technology Stack)"
description: "谈谈我对技术栈这个名词的理解和一些想法"
date: 2018-01-25
tags: [Java, Tech, Talk]
categories: [Tech]
---

> IT 圈是个更新频繁的圈子，时不时就会出现一些新的东西。这些东西或许是一种新的技术，而这些技术的出现或许是某个组织或个人对现状不满，或者就是因为消息闭塞，不知道别人已经做了这些东西而重复做一样的事情；这些东西亦或者是一种思想、理念，说到思想或者理念，这个概念就有点飘忽，是感觉不到实体的东西，其实说白了就像是各种养身大师告诉你该怎么喝水一样，一些养身大师告诉你水要烧开了喝，有的大师告诉你水不能烧，会丢失其中的天然元素等等；这些东西更多的时候是一个名词，而伴随这个名词，也会渐渐衍生出一些思想或者技术的新东西。

### Contents
* [Preface][preface-contents-link]
* [Technology Stack][technology-stack-contents-link]
* [Full Stack][full-stack-contents-link]
* [Reference][reference-contents-link]

---

[preface-contents-link]: #1-preface
[technology-stack-contents-link]: #2-technology-stack
[full-stack-contents-link]: #3-full-stack
[reference-contents-link]: #4-reference


### 1. Preface

正如开篇引言里所说的，IT 圈一些新的东西都是从新的名词开始，那或许可以先从这些新的名词开始谈起。至于这个“新”字，在 IT 领域中，一种情况是表示这个名词是刚刚被发明，并出现在大众视线之内。但其实更多的时候，由于消息的闭塞，或者你所关注领域的不同导致这个“新”字所表示的是你刚听到这个名词。当然还有一种形式，就是这个“新”名词你已经听说过，但对其并不了解，更甚者压根不知道它是什么意思。

今天想要聊的这个名词——`技术栈(Technology Stack)`对于我来说，就是上述的第三种情况，只听过其音，未曾见其形。


### 2. Technology Stack

先从定义上理解这个名词。那何为`技术栈`呢？从字面理解，可以分为`技术`和`栈`，`技术`一词或许比较容易理解，在 IT 领域，可以理解为某个领域一系列知识、工具等的集合。重点在于这个`栈`字。在 IT 领域，栈一般表示为一个数据结构，描述数据如何被存储和读取。更为精确的描述如下wikipedia的词条解释。

>堆栈（英语：stack）又称为栈或堆叠，是计算机科学中一种特殊的串列形式的抽象资料型别，其特殊之处在于只能允许在链接串列或阵列的一端（称为堆叠顶端指标，英语：top）进行加入数据（英语：push）和输出数据（英语：pop）的运算。另外栈也可以用一维数组或连结串列的形式来完成。

用图来更形象地表示如下，数据只能从上边存入，也只能从上边取出。所以`栈`也被叫做`先进后出队列(FILO : First In Last Out)`或者`后进先出队列(LIFO : Last In First Out)`。

![wikipedia关于堆栈的图示][technology-stack-img-1]

看完`栈`的描述，那么`技术栈`和这个数据结构由什么关系呢？根自己所掌握的技术来结合来看，好像没有什么关系。比如你从事 Java Web 开发，那么一般来说，你需要掌握 Java SE 的知识，一些便于开发的 Java 技术框架，比如 Spring 之类的，还有一些前端知识，比如 JavaScript，CSS 等，还有数据库相关的一些东西。那这些东西怎么形成一个`栈`数据结构呢？
或许可以根据一个 Web 请求的流程来看，
```
请求 ：浏览器 -> HTML/JavaScript/CSS -> Spring MVC -> Service(Spring DI ...) -> DB
响应 ：浏览器 <- HTML/JavaScript/CSS <- Spring MVC <- Service(Spring DI ...) <- DB
```
将上述的流程中，浏览器看作`栈`的入口，请求和响应都经过了各个层次之间的协同工作最后成型，再怎么看都是一个链表式结构嘛。完全没有`LIFO`队列的意思。那`技术栈`中的这个`栈`字，就不是 IT 领域中用于描述数据结构的意思。

那`技术栈`是个什么概念呢？以下是来自`百度百科`的词条信息。

>IT术语，某项工作或某个职位需要掌握的一系列技能组合的统称。technology stack 技术栈一般来说是指将N种技术互相组合在一起(N>1)，作为一个有机的整体来实现某种目的。也可以指掌握这些技术以及配合使用的经验。

按这个描述，`技术栈`就是一堆技术的`组合`。而这里的`栈`不是数据结构，只是表示这个`组合`。那这一堆技术，或许有关联，或许没有关联。这些技术之间的关系要是非要用一种数据结构来描述，那`树`或者`图`更为贴切。但恰恰是用了`栈`这个字眼来描述，让人难免有点误解。

---

[technology-stack-img-1]: {{ site.url }}/assets/posts_img/technology-stack/Data_stack.svg


### 3. Full Stack

既然`技术栈`这个名词有了一定的了解，顺便再来聊聊`全栈(Full Stack)`。所谓全栈工程师，按百度百科的词条解释，是`掌握多种技能，并能利用多种技能独立完成产品的人`。听起来很不错，但细细想想，是否真的如听起来那般好呢？在我看来其实不然，因为个人的时间和精力十分有限，每个领域的东西又庞杂繁琐，想要学以致用是个很浪费时间和精力的事情，更别说精通掌握。百度百科关于`技术栈`的词条中，也有一笔`全栈`的描述：`Full Stack（全栈），简单地说是万金油，说得体面一点就是前端、后台、存储、架构等都懂。`。其实，某种程度上，我同意这种说法。对于各种技术仅限于`懂`的层次，或许简单地了解和基本使用可以完成工作中大部分工作，但当出现莫名其妙的问题时，将会束手无策。从个人发展角度来说，自己缺乏一个核心竞争力的体现。

关于`全栈`的个人看法先说这么多吧，或许之后会有精力时间来做个更深层次的解读。


### 4. Reference

* [百度百科`技术栈`词条][reference-link-1]
* [wikipedia关于`stack`的词条(请科学上网)][reference-link-2]
* [谁能告诉我技术栈是什么东东？][reference-link-3]
* [百度百科`全栈工程师`词条][reference-link-4]

---

[reference-link-1]: https://baike.baidu.com/item/%E6%8A%80%E6%9C%AF%E6%A0%88/20152817?fr=aladdin
[reference-link-2]: https://zh.wikipedia.org/wiki/%E5%A0%86%E6%A0%88
[reference-link-3]: https://segmentfault.com/q/1010000002421490
[reference-link-4]: https://baike.baidu.com/item/%E5%85%A8%E6%A0%88%E5%B7%A5%E7%A8%8B%E5%B8%88

