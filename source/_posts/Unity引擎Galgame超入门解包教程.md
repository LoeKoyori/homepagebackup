---
title: Unity引擎Galgame超入门解包教程
author: Loe-Koyori
avatar: https://cdn.jsdelivr.net/gh/LoeKoyori/cdn@2.0/img/custom/avatar.png
authorLink: https://blogdemo.playmarxcards.online
authorAbout: 一个在学习如何思考的人
authorDesc: 一个在学习如何思考的人
categories: 杂谈
date: 2021-4-24 23:05:20
comments: true
tags: 
 - 随笔
keywords: Unity 
description: 应用开源项目AssetStudio
photos: https://ww1.sinaimg.cn/large/007gk3UJgy1gqr9o39u46j31z41401fu.jpg
---

4月23日，**素敵な彼女の作り方**（可爱女友的获取方法）发售。我不是百合豚，也不怕踩雷，想着最多把本作当灾难片看，于是第二天就把本作通了。即使你不怕吸毒、男性、反向PUA这些雷点，本作还是不太行。本作是个悲剧，算是把人生有价值的东西毁灭给人看，但在剧情中又体现不出编剧的任何思考。最后一个选择支，选项上有一大堆代码，但最后却与 metagame 没有关系，只是为了表现女主精神状态不稳定。本作纯属为了猎奇而猎奇，我猜测是编剧被强塞了一大堆诸如 metagame、病娇、全be、嗑药之类的元素，只好无机地把这些元素强揉在一起，酿成剧本灾难。Bangumi的4.2评分很客观地反映了本作的水平。

不过，今天发售的**9-nine-新章**还算可以，毕竟日后谈也不太可能崩。拿这作洗洗眼睛后感觉好多了。

说回正题，素敵な彼女の作り方 的主菜单 bgm 给我留下了挺深的印象。一方面，其本身的悬疑感、诡异感恰到好处；另一方面，我每次打完 Bad End 一脸迷惑、目瞪口呆时，跳转回主界面后自动播放的 bgm 与我此时的心境总能**相得益彰**。于是，我决定把本作的 bgm 提取出来。

下面，以解包本作为例，讲解一下**如何解包 Unity 引擎的 Galgame**。

## AssetStudio

今天用到的工具是开源项目是 **AssetStudio**。AssetStudio 是一个对用户友好的图形化工具，可用于 unity 项目素材的提取、导出。

项目地址  [Perfare / AssetStudio](https://github.com/Perfare/AssetStudio)，如不能访问 Github ，可使用本站Koyori搭建的 [Perfare / AssetStudio](https://proxy.playmarxcards.online/-----https://github.com/Perfare/AssetStudio)。

在 [Release](https://github.com/Perfare/AssetStudio/releases/tag/v0.15.0) 页面下载，代理地址 [Release](https://proxy.playmarxcards.online/-----https://github.com/Perfare/AssetStudio/releases/tag/v0.15.0)

解压后运行目录内**AssetStudio.GUI.exe**

![unity1.png](http://ww1.sinaimg.cn/large/007gk3UJgy1gqra5o8uwrj30il0f6myz.jpg)

打开后点击**File - Load Folder** 载入游戏文件夹

![unity2.png](http://ww1.sinaimg.cn/large/007gk3UJgy1gqra84irz5j30z60nuq3a.jpg)

载入成功后点击** Asset List** ，就可以看到游戏用的素材了

![unity3.png](http://ww1.sinaimg.cn/large/007gk3UJgy1gqraacsbr7j30yt0jhgxx.jpg)

通过点击 Name 或 Type 进行排序，可以更轻松地找到想要的资源。右键点击文件（按住 shift 可批量选取），点击 **Export selected assets** 即可导出。也可使用 **Export** 菜单进行导出。

