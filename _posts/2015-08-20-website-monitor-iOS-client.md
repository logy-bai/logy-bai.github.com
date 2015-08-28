---
layout: post
title: "网站监控iOS客户端"
description: ""
category: iOS
tags: [iOS]
---
{% include JB/setup %}
###iOS客户端源代码
[here](https://github.com/logy-bai/Website-Monitor)
###服务器端的程序
服务器端维护一个被监控网站的列表，然后对列表中的每个网站固定间隔的轮询，当某个网站出现无法访问时，再请求一次改网站，确认无法访问后改变数据库中该网站的状态，同时将监控该网站的频率减小至2分钟一次。当该网站恢复正常时，再将监控频率恢复至正常，并计算网站的无法访问的总时间。
###客户端程序的实现

1. 使用多组数据，每个网站条目算是一组（section）。
2. 自定义化每组数据的HeaderView，并添加点击手势识别。
3. 默认情况下，每组下的行数(row)为0.
4. 当点击headerView时，为该section添加一行，行的内容就是网站的详细情况。
5. 再次点击header时，就删除该section的一行数据，从而实现tableView的下拉菜单功能。

###学习到的知识点
1. 更加熟悉了objective-c的面向对象编程。
2. 复习了UINavigationController，UITableView的使用，单例模式的使用。
3. 动手实现自定义的HeaderView
4. 

###下一步的打算
1. 把数据固化。假如没有联网，就会显示最后一次请求的数据。```（因为数据量较小，不需要保存历史数据，所以不采用CoreData）```
2. 加入APNS,当有新的网站无法访问，或者网站恢复正常，推送提示
3. 美化外观
