---
title: 讲讲用BotUI中碰到的几个坑
author: Loe-Koyori
avatar: https://cdn.jsdelivr.net/gh/LoeKoyori/cdn@2.0/img/custom/avatar.png
authorLink: https://blogdemo.playmarxcards.online
authorAbout: 一个在学习如何思考的人
authorDesc: 一个在学习如何思考的人
categories: 技术
date: 2021-5-14 21:35:00
comments: true
tags: 
 - web
keywords: BotUI
description: 写bug一时爽，找bug火葬场
photos: https://ww1.sinaimg.cn/large/007gk3UJgy1gqkduymjf9j30zk0p8dp1.jpg
---
在写本站的 [about](https://blogdemo.playmarxcards.online/about/) 页面时，用到了一个开源的项目 [BotUI](https://docs.botui.org/) 。通过 **BotUI** ，可以比较容易地实现聊天界面。它的api也特别简单，十几分钟看一遍官方文档就基本能写了。 

本来初稿的文稿量比较大，分支也比较多，但考虑到这界面又不能 SL (Save and Load)，其实不适合用来大量获取文本。最后我决定把稿子中严肃的部分提取出来单独放进文章 [Hello World](https://blogdemo.playmarxcards.online/1926/08/17/Hello%20World/) 里，[about](https://blogdemo.playmarxcards.online/about/) 页面稿子精简到300字，总体基调轻松一些。

最后的成品虽然表面上看起来是有分支，但其实还是线性的。在“我”说完前三句话时，用户迎来第一个分支：“然后呢”或“少废话”。

![图片](http://ww1.sinaimg.cn/large/007gk3UJgy1gqkcqblspvj30m00aw3yu.jpg)

其中，如果选择“然后呢”，“我”将会推进对话；如果选择“少废话”，“我”会发一张图和一句话，然后强制选择“然后呢”，再继续推进对话。

![图片](https://ww1.sinaimg.cn/large/007gk3UJgy1gqkcu37k9qj30kw0aeabu.jpg)

这个图片是用 **markdown** 语法实现的：

```javascript
end = function () {
    botui.message.add({
        delay: 800,
        content: '![...](https://*.gif)'
    }).then(function() { 
        botui.message.add({
            delay:1500,
            content: "哼，再给你一次组织语言的机会💘"
        }).then(function () {
            botui.action.button({
                delay: 1200,
                action: [{
                    text: "然后呢？ 😱",
                    value: "sure"
                }]    
            }).then(function () {
                    secondpart()
            })
        })
    })       
}
```

但是测试下来，图片并不会显示出来，而会出现一个空白的消息框。打开浏览器控制台，发现图片资源确实被下载了，但消息框内 *context* 为空。如果在显示图片后直接使用`botui.message.button` ，则可以正常显示。但在点击按钮后消息框又变为空白。

在之后还有一个用到 markdown 语法表示链接的，但也是显示文字不能表示链接。

经确认，我用的 **BotUI** 的版本确实是最新。查了很久，发现问题在这块代码：

```javascript
root.Vue.directive('botui-markdown', function (el, binding) {
      if(binding.value == 'false') return; // v-botui-markdown="false"
      el.innerHTML = _parseMarkDown(el.textContent);
});
```

这导致只要下一消息一被显示，上一使用 markdown 语法的消息就会被立即隐藏。`botui.message.add`的`delay`是先于消息展示的，所以上衣消息在显示图片的瞬时，就会进入下一消息的等待时间，消息就会被立即隐藏。

解决方法是将上方代码块修改一下：

```javascript
root.Vue.directive('botui-markdown', function (el, binding) {
    if(binding.value == false || el.getAttribute('botui-markdown-done')) return; // v-botui-markdown="false"
    el.innerHTML = _parseMarkDown(el.textContent);
    el.setAttribute('botui-markdown-done', true); // mark the node as already parsed
});
```

这样就可以正常显示了。

比较可惜的是，**BotUI** 好像已经两三年没人维护了，估计也没什么再更新代码的可能性。

