---
layout:     post                    # 使用的布局（不需要改）
title:      几步操作，使用Github pages+jekyll搭建自己的博客              # 标题 
subtitle:   小白踩坑日记，亲测可行 #副标题
date:       2018-04-12             # 时间
author:     WZH                      # 作者
header-img: img/post-bg-2015.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - test
---

## 搭建博客指南
从一无所有到成功搭建出一个blog，历程可以说是痛苦的。最开始自己也从网上搜了很多资料，例如阮一峰老师的[搭建一个免费的，无限流量的Blog----github Pages和Jekyll入门](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html),自己看了半天还是云里雾里。里面涉及了“很多”命令行操作，同时，这篇文章距离现在实在是···过于久远，所以看完了、跟着敲了命令也收获甚少。
于是自己在百度直接搜索[GitHub Pages](https://pages.github.com/) + [jekyll](http://jekyll.com.cn/)，收获还是挺多的。具体教程大同小异，先奉上感觉写的最好的一篇——[BYQiu](http://qiubaiying.top/)的[利用 GitHub Pages 快速搭建个人博客](https://www.jianshu.com/p/e68fba58f75c),这片教程写的非常详细，大家可以按照步骤一步一步来,基本能够成功搭建出github Pages的个人博客。但是，俗话说>人无完人，这篇教程中也存在一个坑——关于仓库名的，如下图：
![](https://cl.ly/22242t2K1z3Y)
对于这个404报错，这里就只说了一句“检查仓库名”。由于之前看过很多篇教程，几乎每一篇教程都提到过要把fork下来的仓库名称改为**用户名.github.io**的形式，所以我也理所当然的回去检查这个仓库名，甚至还切换大小写中引文，然并卵···折腾半天也没解决，想了想还是决定问一下度娘。于是我便把404报错页面的错误提示粘贴到*百度*上，终于找到一篇[github.io DNS CNAME 解析 There isn't a GitHub Pages site here. 记录](https://blog.csdn.net/aloh_a/article/details/78963998)(再次感谢各位先行者的分享!),这才明白原来fork下来的还有个**CNAME**文件需要修改，前面说的检查仓库名应该指的是这个文件中的仓库名。
![](https://cl.ly/302n1k0C1C3W)
在这个文件中将仓库名改为**用户名.github.io**的形式再去setting中查看时，会提示**用户名.github.io**的形式是默认的，所以只需要将**CNAME**文件清空就行了。之后再跟着教程来做就没有什么问题了。
最后再次感谢[BYQiu](http://qiubaiying.top/)的分享。



