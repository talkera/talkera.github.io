---
title: Web Notification 不打开页面就能收到推送
date: 2018-04-21
categories: 其他
tags: [web notification]
---
打开浏览器后收到FB推送的一条消息，而浏览器里此时根本就没有打开FB的页面。

<!--more-->
## 打开页面才能收到的通知
这是网页版微信有的功能，允许通知后即便最小化浏览器，有新消息时还是能收到提醒。用到的就是 web notification

Notifications API 允许网页或应用程序在系统级别发送在页面外部显示系统层面的通知。通常使用操作系统的标准通知机制，但是在不同的平台和浏览器上的表现会有差异。最明显的用例之一是一个网页版电子邮件应用程序，每当用户收到了一封新的电子邮件都需要通知用户，即使用户正在使用另一个应用程序。


[PUSH API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)

[web notification](https://developer.mozilla.org/zh-CN/docs/Web/API/notification/Using_Web_Notifications)

[W3C关于web notification的标准](https://www.w3.org/TR/notifications/)
