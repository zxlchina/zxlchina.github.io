---
layout: post
title:  "如何在SVN中使用外链"
date:   2016-07-21 21:42:30 +0800
categories: svn externals
---

笔者在工作中遇到需要在SVN的一个仓库中使用另一个SVN仓库中的文件，刚开始通过拷贝文件，刚开始还感觉比较方便，但是久而久之维护成本越来越高，经常会因为两边文件不一致导致的各种问题。为了解决文件同步的问题，这里使用了SVN的外链。

SVN外链是建立引用指向仓库内或者其他仓库的文件或者文件夹。譬如有A、B两个仓库，A仓库中有大量协议描述文件，B仓库需要用到A中的描述文件，那么我们就可以在B仓库建立一个外链指向A仓库的指定文件夹，当A仓库中的文件更新后，只需要update B仓库就可以自动更新了。外链的类型分为**外部文件夹**和**外部文件**之分。

* 外部文件夹
在需要建立外部文件夹的目录中之行如下指令：

```
svn propedit svn:externals .
```

进入外链编辑，按照如下格式输入：

```
name URI
lib https://svn.example.com/Framework/trunk/lib
```
保存后，执行:

```
svn up
```

即可将https://svn.example.com/Framework/trunk/lib文件夹同步到当前目录下的lib下

* 外部文件
同步文件夹粒度毕竟有些粗糙，很多时候我们只希望引用指定目录下的某几个文件而不是全部文件夹，此时外部文件就可以派上用场了。外部文件的用法和外部文件夹一样的，只是URI使用的是指定文件的URI。
外部文件在SVN1.6版本以后才支持，同时，只能引用相同仓库的文件，也就是说我们没法直接引用一个外部仓库的指定文件。

那么如何引用外部仓库的单个文件呢？这里将外部文件夹和外部文件结合起来就可以很好解决该问题了。

1. 在A仓库中新建一个extern文件夹
2. 在extern文件夹下分别建立引用，指向需要共享的文瑾啊
3. 在B仓库中需要使用共享文件的地方，建立引用指向A仓库中的extern文件夹即可 
