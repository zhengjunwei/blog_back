---
title:最清晰的ios消息推送机制教程(转)
layout: page
category: others
date: 2014-06-05
---
#最清晰的ios消息推送机制教程(转)
研究了一下Apple Push Notification Service,实现的很简单,很环保.原理如下

财大气粗的苹果提供了一堆服务器,每个ios设备和这些服务器保持了一个长连接,ios版本更新提示,手机时钟校准什么的都是通过这个连接.

苹果把这个长连接开放出来给大家推送消息用,很积德,因为这是个全球服务,几十亿台ios设备,服务器少说也需要上万台,还没有钱可以赚. andorid的爸爸就不做这个,于是各个app为了发消息,只能直接拼命赖在后台维持一个长连接,电就是这样被耗光的

苹果提供消息服务简称为APNS,只是是长连接机器的一部分,你要向你的用户发消息,必须通过apns中转,你写程序发给它,它转发给你的手机,你的推送程序和用户手机没有直接联系

消息推送不支持群发,只能一个一个发.如果你的App有100万个用户,要发消息怎么办? 一个一个的发呗,发100万次.消息包大概包括两部分:标示用户手机的id(32个字节)+消息体(<=256Bytes),消息体是json字符串,传输过程用ssl加密的

标示用户手机的ID 叫做 device tokens,每个手机都不一样,deviceToken非常重要

device tokens
device tokens每个机器都不一样,比较独一无二,但是不是硬件码,如果你重装了ios系统,可能会发生变化.其实 device tokens 也是用户的手机发起请求,由apns生成的,可以相信,apns后台有一个key-value数据库.

获取device tokens 很简单,只需要实现下面这个函数

(void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken

在这个函数里面,你把deviceToken保存到你服务器上即可,这个函数是个call back函数,ios从apns得到deviceToken,就传给它,你需要做的,写一段保存 这个token的代码

注意:苹果没有承诺deviceToken的生成机制,随时可能变化,最好的方法是你第一次获取到deviceToken之后,也提交,然后存在本地,之后每次都比较,发现有变化,就更新你的服务器上的记录

app支持推送的技术实现
要实现推送功能,你需要干如下几件事情

你需要写3段程序
到苹果开发者中心注册一次,并下载一份cer文件
从苹果的Provisioning Portal,填写并下载一个Provisioning Profile
先说2,3两点

下载cer文件,是推送程序要用,因为要通过ssl信道发送数据

填写并下载Provisioning Profile,并从xcode加入到你的app项目文件中,你可以理解为办手续,总不能无证乱发吧,

需要写的3段程序分别是

前文提到的保存device token的代码,很简单的,随便搭个http服务,用mysql建个表,你在app里面用http post提交就行

第二段程序是:你的app必须做个标记,告诉ios,你会给用户产生推送消息,这个代码很简单,一句话搞定

[[UIApplication sharedApplication] registerForRemoteNotificationTypes:
(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];

加在app初始化函数里面即可

最后是:推送程序,这个代码量可能最多.逻辑很简单:遍历你的存放devictTokent的数据表,逐一发消息给苹果的APNS服务器.推送程序,有很多开源代码,用APNS为关键词,一搜一大把,各种语言都有,改改就能用

苹APNS服务器地址:gateway.push.apple.com,端口是 2195

以前看到有人吹嘘自己100万用户规模消息推送,这个有技术含量么? 就是1000万用户也得一个一个都发完,多进程?长连接?epoll? 能发多快,苹果说了算