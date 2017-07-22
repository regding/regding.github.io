---
layout: post
title: "使用 Zsh 作为 Ubuntu 的默认 Shell"
description: "在 Ubuntu 中使用 Zsh 作为默认 Shell"
date: 2017-07-22
tags: [Zsh, Ubuntu, oh-my-zsh]
categories: [Tech]
---

> Linux 发行版一般都是用 Bash Shell 作为默认的交互 Shell，Bash 提供了简洁的交互方式，以及足够多的工具用于完成任务。那么为什么要将默认交互 Shell 换成 Zsh 呢，其实是为了有一个可以友好支持 Git 的界面，以及更为简洁智能的交互方式，当然，还要美观。

### Contents
* [Preface][preface-contents-link]
* [Zsh][zsh-contents-link]
* [oh my zsh][oh-my-zsh-contents-link]
* [agnoster][agnoster-contents-link]
* [Reference][reference-contents-link]

---

[preface-contents-link]: #1-preface
[zsh-contents-link]: #2-zsh
[oh-my-zsh-contents-link]: #3-oh-my-zsh
[agnoster-contents-link]: #4-agnoster
[reference-contents-link]: #5-reference


### 1. Preface

Ubuntu 的默认 Shell 是 Bash，看起来如下图这样

![Ubuntu默认Shell][ubuntu-zsh-img-1]

其实也已经用习惯 Bash 了，足够简洁简单，简单几行命令就搞定了想要干的事情。但为什么要换 Zsh 呢？我想是因为如下这张图。

![agnoster主题][ubuntu-zsh-img-2]

这张图来自 oh my zsh 主题列表页，而这正是给 oh my zsh 用的主题之一，就是稍后会介绍的 agnoster。
作为 Git 死忠粉，很是需要一个简洁优雅的 Git 界面，在 Windows 和 Mac 中，有 SourceTree，但在 Linux 下，却没有 SourceTree 的支持。
自从看到这个主题对于 Git 的支持，觉得这个就是我想要的。于是才有了想将这个用于 oh my zsh 的主题用于我 Ubuntu 机器的想法。

介绍下 Zsh，oh my zsh，agnoster 是什么，以及他们什么关系。

* [Zsh][zsh-link]
> [Zsh][zsh-link] 是一款功能强大终端（Shell）软件，既可以作为一个交互式终端，也可以作为一个脚本解释器。它在兼容 Bash 的同时 (默认不兼容，除非设置成 emulate sh) 还有提供了很多改进，例如：
> * 更高效
> * 更好的自动补全
> * 更好的文件名展开（通配符展开）
> * 更好的数组处理
> * 可定制性高

* [oh my zsh][oh-my-zsh-link]
> [oh my zsh][oh-my-zsh-link]是基于 Zsh 的功能做了一个扩展，方便的插件管理、主题自定义，以及漂亮的自动完成效果。

* [agnoster][agnoster-link]

[agnoster][agnoster-link]是一个用于 oh my zsh 的主题，作者本是建议给 Mac 用户用的，毕竟 Mac 有漂亮的界面以及 iTerm2 这样好用的终端工具，搭配起来就会更为漂亮高效。以下是 agnoster 官方 GitHub 的介绍。
> A ZSH theme optimized for people who use:
> * Solarized
> * Git
> * Unicode-compatible fonts and terminals (I use iTerm2 + Menlo)
> For Mac users, I highly recommend iTerm 2 + Solarized Dark

接下来，就是在 Ubuntu 安装使用了 agnoster 主题的 Zsh 流程。简要步骤就是：

Zsh -> oh my zsh -> agnoster -> done.

---

[ubuntu-zsh-img-1]: {{ site.url }}/assets/posts_img/ubuntu-zsh/ubuntu-zsh-1.png
[ubuntu-zsh-img-2]: {{ site.url }}/assets/posts_img/ubuntu-zsh/ubuntu-zsh-2.png

[zsh-link]: http://zsh.sourceforge.net/Intro/intro_1.html
[oh-my-zsh-link]: http://ohmyz.sh/
[agnoster-link]: https://github.com/agnoster/agnoster-zsh-theme


### 2. Zsh

Ubuntu 发行版默认是未安装 Zsh 的，需要用户自己手动安装，安装步骤如下：


1. 安装 Zsh

    ```bash
    $ sudo apt-get install zsh
    ```

2. 验证是否安装成功

    ```bash
    $ zsh --version
    ```

3. 将 Zsh 设为默认 Shell

    ```bash
    $ sudo chsh -s $(which zsh)
    ```

4. 注销当前用户重新登录

5. 验证当前默认 Shell
    ```bash
    $ echo $SHELL
    ```
    若输出为 `/bin/zsh` 或者 `/usr/bin/zsh` 则表示当前默认 Shell 是 Zsh

---


### 3. oh my zsh

Zsh 本身作为一个功能强大的 Shell，其配置操作较为复杂繁琐，所以有了 oh my zsh 这个东西，可以更为简单，方便地配置 Zsh，以及管理其插件，主题等，接下来就是安装 oh my zsh。

可以通过以下命令行来自动安装

1. 通过curl
    ```bash
    $ sh -c \
    "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
    ```
3. 通过wget
    ```bash
    $ sh -c \
    "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
    ```

安装完之后，在 `~` 目录下就能看到一个 `.oh-my-zsh` 的隐藏目录，该目录本身还是个 Git 工作目录，当 oh my zsh 有更新时，你登录后会提示你是否需要更新，只要同意更新就好，也可以自主更新，使用如下命令行
```bash
$ git pull origin master
```

安装完 oh my zsh 之后，使用的是默认的主题，如下图
(由于我的环境已经改了主题，借用[Ubuntu 下安装oh-my-zsh][reference-link-1]的截图)

![oh my zsh默认主题][ubuntu-zsh-img-3]

---

[ubuntu-zsh-img-3]: {{ site.url }}/assets/posts_img/ubuntu-zsh/ubuntu-zsh-3.png


### 4. agnoster

前边的步骤已经把 Zsh 以及 oh my zsh 安装好了，现在就开始实现我想要的效果，使用 agnoster 主题，来搭建一个对 Git 友好的命令行界面。

oh my zsh 的配置文件是 `~/.zshrc`，其实这个应该是 Zsh 的配置文件，应该和 Zsh 的全局的配置文件（`/etc/zsh/zshrc`）保持一致，但 oh my zsh 做了修改（这一点是我根据 Bash 的使用经验猜测的）。

为了使用 agnoster 主题，修改 `~/.zshrc` 文件如下图

![修改.zshrc文件][ubuntu-zsh-img-4]

配置完成之后重启终端，应该会出现乱码，如下的图（还是没有图，再次借用[Ubuntu 下安装oh-my-zsh][reference-link-1]的截图)）

![中文乱码][ubuntu-zsh-img-5]

这是因为没有支持 Powerline 的字体，所以需要下载一个支持 Powerline 的字体。

下载[UbuntuMono][UbuntuMono-link]字体，然后双击安装

修改终端的默认字体，改为刚才安装的 UbuntuMono 字体，如图

![UbuntuMono字体][ubuntu-zsh-img-6]

再次打开终端，就可以看到正常显示了，如图（再次借用图）

![正常显示][ubuntu-zsh-img-7]

其实到这一步就可以结束了，但想要更简洁，就想把箭头前边的 `用户名@主机名` 去掉，就需要再改动下配置文件，达到更为简洁的目的


*进一步优化*

看到好多教程都说添加 `export DEFAULT_USER="username"` 到 `~/.zshrc` 文件中，然后重新读取一下 `~/.zshrc` 文件即可。注意，此处的 `username` 就是你的用户名。但在我环境中并未生效，所以我看了下 agnoster 主题的源码，发现其实还可以用个更简单粗暴的方法。

找到 agnoster 主题代码的路径如下

![agnoster代码路径][ubuntu-zsh-img-8]

来看下代码的第 82 行这个 if 判断，前半句判断就是判断 `$USER` 变量和 `$DEFAULT_USER` 是否不同，不同就运行判断体内的语句，大意就是在箭头前加 `user@hostname` 之类的

![agnoster代码][ubuntu-zsh-img-9]

既然知道了是什么逻辑，那就简单了，直接把 if 语句里的那句注释掉就好了

![git diff][ubuntu-zsh-img-10]

到此，在 Ubuntu 上搭建使用 agnoster 主题的 Zsh 环境已经完成，来看下成品效果。

![成品图][ubuntu-zsh-img-11]

---

[ubuntu-zsh-img-4]: {{ site.url }}/assets/posts_img/ubuntu-zsh/ubuntu-zsh-4.png
[ubuntu-zsh-img-5]: {{ site.url }}/assets/posts_img/ubuntu-zsh/ubuntu-zsh-5.png
[ubuntu-zsh-img-6]: {{ site.url }}/assets/posts_img/ubuntu-zsh/ubuntu-zsh-6.png
[ubuntu-zsh-img-7]: {{ site.url }}/assets/posts_img/ubuntu-zsh/ubuntu-zsh-7.png
[ubuntu-zsh-img-8]: {{ site.url }}/assets/posts_img/ubuntu-zsh/ubuntu-zsh-8.png
[ubuntu-zsh-img-9]: {{ site.url }}/assets/posts_img/ubuntu-zsh/ubuntu-zsh-9.png
[ubuntu-zsh-img-10]: {{ site.url }}/assets/posts_img/ubuntu-zsh/ubuntu-zsh-10.png
[ubuntu-zsh-img-11]: {{ site.url }}/assets/posts_img/ubuntu-zsh/ubuntu-zsh-11.png

[UbuntuMono-link]: https://github.com/powerline/fonts/tree/master/UbuntuMono


### 5. Reference

在自己安装过程，多多少遇到了一些问题，比如安装完 agnoster 主题后，在终端打开会出现乱码，多亏万能的 Google 和各位大神的 Blog 分享，才得以顺利解决我的问题。以下是本博客参考到的文档链接。

* [Ubuntu 下安装oh-my-zsh][reference-link-1]
* [Zsh中文介绍][reference-link-2]
* [oh my zsh中文介绍][reference-link-3]

---

[reference-link-1]: http://www.jianshu.com/p/9a5c4cb0452d
[reference-link-2]: https://wiki.archlinux.org/index.php/Zsh_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)
[reference-link-3]: https://www.oschina.net/p/oh-my-zsh


