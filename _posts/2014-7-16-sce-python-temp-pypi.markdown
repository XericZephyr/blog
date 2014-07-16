---
layout: post
title:  "Python伪开发者对于搜狐云景的测评"
date:   2014-7-16 17:11:00
categories: python unicode
---

#初次见面

本人是GAE和OpenShift的狂热爱好者，玩过各种国外PaaS。某次想搞个稍微复杂点的Python Web程序，需要比较好的网络传输速度，就试图找前PM(Project Manager)要个国内的VPS耍一把。前PM表示近来搞了个搜狐云景的公测激活码，让我先试试，于是就有了我在SCE的第一个奇怪的Python应用。

PS: SCE是搜狐云景是搜狐公司自主研发的与语言无关、可提供弹性伸缩服务的公有云PaaS平台，现致力发展成为最开放的PaaS平台。 （无责任Copy自SCE官方微博）

# 吐槽在最前面

我第一个Python应用还是很简单的，仅有几十行代码的Restful API，用的Web框架是Flask，轻松加微笑。
部署到SCE的时候发现SCE的Python没有Flask模块，也没有在app.yaml配置文件中提供类似 require 之类的字段后台自动安装。 （据技术群里的管理说这个功能马上上线）

# 通向Flask的艰辛之路

## SSH + easy_install 大法

壮哉我大Flask。可惜SCE官方没有支持。怎么办呢。。。
所幸我发现了SCE支持SSH这个神奇的东西，于是Putty搞了一把之后发现了Python目录，然后里面有easy_install。。。试了一下之后。。。
哈哈哈。。。Bingo

## 代码包包含大法

代码包中直接包含Flask也是个不错的方法，主要是在代码包中包含`/lib/`文件夹，然后用
`pip install Flask -t lib/`
直接将包安装到代码包中去，仅对纯Python包有效，对包含其他语言编译的pypi包无效。

PS: 我应该会在github上开源一个sce-python-flask-skeleton，稍后给出链接。

# 好吧， SCE还是优点的。。。

## Git 部署

虽然很多人更适应SVN（国内云大都采用的方式，简单粗暴），可是我作为一个GitHUB玩家和OpenShift的用户，还是最习惯Git部署。鄙人一直都觉得`git push origin master` 之后端一杯咖啡看着 `git shell`后面一行行跳出来的部署Log，才是轻松惬意的程序员生活。虽然SCE暂时还没有那一行行的部署Log。 ╮(╯_╰)╭

## SSH 访问

SCE的容器（官方叫法是“实例”，某种程度上被阉割比较厉害的VPS）竟然是支持SSH访问的，虽然连ping命令都被阉割掉了。可以easy_install，还可以curl。有兴趣的童鞋们请继续YY。。。（PS: 我已经成功带坏了技术群里的另外一人，你们什么都不知道不要告诉别人。。

## 多语言支持

SCE支持的语言貌似很多，Java, Php, Python, Ruby, Lua, NodeJS(无责任从官网Copy来的)。国内某以A开头（邪恶的笑）的云引擎仅支持Java和PHP，然后装个Drupal都第一步失败，被我直接弃掉了。

# 总结

我还是很喜欢SCE这种类似OpenShift的非常灵活的PaaS的，没有像传统PaaS（类似于GAE,SAE）那么复杂的限制和独立的API（移植成本太高），也没有像普通IaaS那样高昂的价格和复杂的配置。也许是我更喜欢这种自己一点点挖掘，探索发现的感觉吧。