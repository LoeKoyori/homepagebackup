---
title: 使用 Moyea SWF to Video Converter 实现 swf 文件转换 mp4 文件
author: Loe-Koyori
avatar: https://cdn.jsdelivr.net/gh/LoeKoyori/cdn@2.0/img/custom/avatar.png
authorLink: https://blogdemo.playmarxcards.online
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 技术
date: 2021-5-13 14:18:01
comments: true
tags: 
 - 教程
keywords: SWF
description: swf文件转换mp4文件
photos: https://ww1.sinaimg.cn/large/007gk3UJgy1gqgrviycw6j31z4140kfr.jpg
---
前几天，父母给我发了一条消息和两个 swf 文件。我很惊奇：这都2021年了，怎么还有 swf 文件呢？一问原因，原来妹妹的学校还在沿用上古时期传下来的课件，而老师也只在家长群里发了 swf 文件，结果家长们纷纷打不开，父母便向我求助。

## swf是什么？

SWF 是 **Small Web Format** 的缩写, 读作**swiff**），是用于多媒体、矢量图形和 ActionScript 的 Adobe Flash 文件格式。很可惜，这个东西是基于 Flash 的。2012年，Adobe 就宣布 Flash 播放器不再支持安卓，尽管实际上能通过安装一些第三方软件实现播放swf，但~~以家长群家长如此有限的技术水平~~也难以推广。pc就更不用说了，这年头哪有家长肯让小学生看电脑？

## swf 转 mp4

想来想去，要让可怜的家长们给家里的小学生看完老师下发的动画，就只能把它转成通用的视频格式。值得一提的是，百度误人子弟的能力仍是一流。以下是百度搜索  *手机打开swf*  排名第一的搜索结果。

![什么叫掩耳盗铃啊？](https://exp-picture.cdn.bcebos.com/836a6aee1c324b186e62014453a72633498448e7.jpg?x-bce-process=image%2Fresize%2Cm_lfit%2Cw_500%2Climit_1%2Fquality%2Cq_80)

嚯，改个扩展名就是 mp4 了，那安装 Linux 是不是把 Windows 镜像改名成 Linux 就可以了啊？

说回正题，最后找到了 **Moyea SWF to Video Converter** 这款软件，大概原理是实时渲染然后录制。

![Moyea SWF to Video Converter](http://ww1.sinaimg.cn/large/007gk3UJgy1gqg24z3zy1j30hc09vdi8.jpg)

前几个月，这款软件还能够正常运转，但一周前的windows更新移除了我原有的 Flash player ，导致这款软件无法正常工作。在网上尝试过很多个版本的 Flash Player ，但都出现了兼容性问题，其中大部分是转换出来的视频没声音。无奈，只好上网搜索解决方案，后来发现这个问题在新版本得到了解决。很遗憾，我的序列号只支持旧版本，新版本只能够试用，输出的视频在正中间有巨大的水印。

当然，本篇文章不是要讲怎么破解它，而是讲如何利用这两个版本实现完整、无水印的 mp4 转换。既然旧版本不能够输出音频，新版本输出的视频有水印，那我们只要把旧版本的输出视频流和新版本输出的音频流合成就可以了。为了做到完全无损的合成，我们用到 **FFmpeg** 。

**FFmpeg** 是一个开放源代码的自由软件，可以运行音频和视频多种格式的录影、转换、流功能。它是几乎目前所有视频压制、剪辑软件的标配。安装 FFmpeg 非常简单，直接去官网下载安装后手动配置环境变量即可，最后可以调用 cmd 输入`ffmpeg -version`，如果能正常显示版本信息就算配置成功了。

![命令行输出](http://ww1.sinaimg.cn/large/007gk3UJgy1gqgrgh0norj30qi089mx9.jpg)

假如你对安装 FFmpeg 有疑问，可以看这个[FFmpeg安装教程](https://www.jianshu.com/p/2b609afb9800)，相信应该没有什么难度。

安装好 FFmpeg 后，在视频输出目录打开 cmd ，输入

```
ffmpeg -i video.mp4 -i sound.mp4 -c:v copy -c:a aac -strict experimental -map 0:v:0 -map 1:a:0 output.mp4
```

其中 video.mp4 是旧版本输出的视频，sound.mp4 是新版本输出的视频。这条命令的作用就是在当前目录生成一个 output.mp4 ，其中的视频流复制自 video.mp4，音频流复制自 sound.mp4 。由于是复制，视频生成速度极快。

最后把输出的视频发到家长群里拯救嗷嗷待哺的家长们就大功告成了！

