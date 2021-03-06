---
layout: post
author: 孙福生
title: 亲身实践Git分支管理
cover: "zzz"
categories: Git
tags: Technology
---

我司以前一直用的是SVN作为代码版本库管理。最近切换到Git上，相比于SVN，Git有很多优点，
其中很显著的一点，就是版本的分支（branch）和合并（merge）十分方便，接下来的内容是自己实际中操作的笔记。


### 首先给大家看的是Git常用命令速查表

<img src="/assets/git_map.jpg" style="width: 100%;">

不用看上面的Git速查表，想必大家也都不陌生吧，是不是大牛！言归正传...


### 为什么要用到分支呢？

我先抛出我司的实际场景，然后你就会知道用Git实在太方便啦。  
我们的线上版本需要维护，可能会出现意想不到的Bug，就需要发小版本进行补救，但我们还在开发新的版本啊，肯定不能在当前代码上修复Bug提交，而如果把上次的工程版本另外保存下来那非常麻烦，好，来了，就使用Git分支解决这个痛点。


### 第一种功能分支，先看下图：

<img src="/assets/git_branch_1.jpg" style="width: 80%;">

我们的开发不是在master主分支上的，是在dev（develop）上，主分支上主要保存各版本稳定的代码，我们不希望把主分支弄坏，所以在dev分支上开发，每个版本的开发也可以从dev分支上打出小的功能（feature）分支，开发完每个feature分支后再合并到dev分支上。

	查看分支  
	$ git branch  

	创建dev分支  
	$ git branch dev  

	将dev分支提交到仓库  
	$ git push -u origin dev  

	切换到dev分支  
	$ git checkout dev  

	也可以从master分支创建完dev后直接切换过去  
	$ git checkout -b dev master

	在dev上完成相关的编码后就是正常的提交到dev分支上啦
	$ git add .  
	$ git commit -m""  
	$ git push origin dev 

	 完成这一版开发后将dev上的代码合并到master分支上，切换到master分支上合并代码  
	$ git merge --no-ff dev  
	（–-no–ff 会执行正常合并，同时在master分支上生成一个节点纪录，这也是我们希望的）  

	确认这一版完成后，我们可以打tag作为标记  
	$ git tag v0.2  
	$ git push --tags  


### 第二种改Bug分支，先看下图：

<img src="/assets/git_branch_2.jpg" style="width: 80%;">

	#这种fixbug分支和dev分支一样，比如从dev分支创建fixbug-v0.2后并切换到fixbug-v0.2分支  
	$ git checkout -b fixbug-v0.2 dev  

	修改完这个Bug后，我们不需要保留它，可以删除这个分支  
	$ git branch -d fixbug-v0.2


提几个注意的点，以后会继续更新这个blog：  
1、我们并不需要克隆一份dev分支的工程代码，有些人可能不知道哦。  
2、

	如果当前在dev分支上，我们要把dev分支上的代码合并到master分支上，需要切换到master分支上并执行
	$ git merge --no-ff dev  相反亦然

### 清除 .git 或 .svn 的配置文件

	$ cd 到工程的根目录 

    清除 .svn 配置文件
    $ find . -name ".svn" | xargs rm -Rf

    清除 .git 配置文件
    $ find . -name ".git" | xargs rm -Rf

