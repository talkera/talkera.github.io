---
title: Web Notification 不打开页面就能收到推送
date: 2018-04-21
categories: 其他
tags: [web notification]
---
打开浏览器后收到FB推送的一条消息，而浏览器里此时根本就没有打开FB的页面。

<!--more-->
## 打开页面才能收到的通知
这是网页版微信就有的功能，允许通知后即便最小化浏览器，有新消息时还是能收到提醒。用到的是 web notification

Notifications API 允许网页或应用程序在系统级别发送在页面外部显示系统层面的通知。通常使用操作系统的标准通知机制，但是在不同的平台和浏览器上的表现会有差异。最明显的用例之一是一个网页版电子邮件应用程序，每当用户收到了一封新的电子邮件都需要通知用户，即使用户正在使用另一个应用程序。

调用web notification只需要通过js获取通知权限，然后新建一个通知即可。

## web push api
真正允许不打开网页也能收到提醒，依赖的是web push api.该协议还是草案状态，但是Facebook已经使用，预计以后变化不大。

## 名词概念
### 简单名词
- webapp: web应用。指运行在user agent中的网站应用
- user agent：web客户端。可以理解成浏览器，也可以指运行web应用的其他web环境。
- push message：推送消息。从应用server发送到web应用的数据。message要被推送到与提交消息的push订阅相关联的active worker
- push service：是一个允许应用server向web应用发送push消息的系统。
- service worker
- application server：提供web应用的server
- push endpoint：推送终端

### push subscription
推送订阅。推送订阅是代表web应用,在user agent和推送server之间建立的消息传递上下文。每一个推送订阅都与一个service worker对应，一个service worker最多只能对应一个推送订阅。
一个推送订阅必须同一个push endpoint关联起来。这必须是push service对外开放的一个绝对URL。应用server可以向push service发送push消息。一个push终端要能唯一标识推送订阅。
一个推送订阅可以设置一个订阅超时时间。超时之前，user agent要尝试更新推送订阅。
如果



## 推送架构
一条消息从应用的server发送到web应用的流程如下：
- 应用server

[PUSH API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)

[web notification](https://developer.mozilla.org/zh-CN/docs/Web/API/notification/Using_Web_Notifications)

[W3C关于web notification的标准](https://www.w3.org/TR/notifications/)
