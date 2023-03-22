---
title: '《Beginning Game Development with Godot Learn to Create and Publish Your First 2D Platform Game》读后感'
date: 2022-08-19
tags: []
slug: lesslessbeginning-game-development-with-godot-learn-to-create-and-publish-your-first-2d-platform-gamegreatergreater-du-hou-gan
featuredImage: /images/lesslessbeginning-game-development-with-godot-learn-to-create-and-publish-your-first-2d-platform-gamegreatergreater-du-hou-gan.png
---

# 0 写在前面

这本是书是关于Godot游戏开发的，书中描述了使用Godot开发一个简单2D平台游戏demo的全流程——从素材导入、代码编码、二进制导出甚至简单地发行自己的游戏都有讲到。
手把手教学，描述非常细腻，极大程度避免了新人出现“环境不对”、“找不到配置项”等琐碎耗时的杂问题。“细腻”，也注定了在有限篇幅内，其内容的有限，但对传统2D平台游戏开发的基础内容都有所描述——场景搭建（tilemap）、基础移动、发射“子弹”击杀敌人以及穿插其中讲解了一些编码逻辑。

<!--more-->

# 1 错误与不足

为了简单，书中不乏一些“低质量”代码，比如玩家血量，为了好实现，一开始是记录在怪物对象上的，乍一看会让人匪夷所思，不过在后续章节修复了这个问题。同时也存在一些失误，在介绍全局变量那里，引导读者把分数和血量记录在全局变量，以便切换关卡的时候记录这些值，但是在玩家血量掉到0死亡之后这些值不会重置，导致血量一直都是0，再次开始的时候，游戏会直接结束。

# 2 关于导出模板下载慢的解决方案

我使用的Godot3.4.4，自动下载导出模板是非常慢的。这里推荐去Godot的[Github仓库](https://github.com/godotengine/godot/releases)直接下载安装，选择对应版本的`Godot_v<你的Godot版本号>-stable_export_templates.tpz`下载，然后在Godot中的“编辑器”——“导出模板管理器”——“选择从文件安装”，选择下载的文件安装即可。

# 3 随想

关于Godot的学习，由于有其他事务，短时间内应该不会再继续看新书了。最近加入了矢量工坊的开源游戏群，里面的程序（几乎）全是外行转码或者初高中小朋友，这里姑且不论好坏。管中窥豹，也可以看到国内独立游戏团队的参差水平和艰难处境。最后，学Godot只是个人爱好，我也完成过Unity的2D平台游戏demo开发，但我并不看好国内的游戏市场。君量力而行，禽择良木而栖。
