---
layout: post
title:  ERROR ITMS-90478, ERROR ITMS-90062
date:   2017-04-27 23:10
categories: iOS
author: asvircc
---

最近助理反馈一个问题，APP 无法连接机器。具体 APP 名字都快没印象了。查看了 APP 更新信息，15年8月最后一次更新。问题还好，很快搞定了。打包上传的时候出错了，给了下面的提示信息

### 问题

- **ERROR ITMS-90478**: "Invalid Version. The build with the version “1.46” can’t be imported because a later version has been closed for new build submissions. Choose a different version number."<br>
- **ERROR ITMS-90062**: "This bundle is invalid. The value for key CFBundleShortVersionString [1.5] in the Info.plist file must contain a higher version than that of the previously approved version [1.46]."

**原因**：其实写的很清楚了，最新的线上版本号 1.46，所以再次提交，版本号要比 1.46 高。1.5 版本号是低于 1.46 版本号的。<br>
**解决**：方法比较简单，把 TAGETS > General > Idntity 中 Version : 1.5 修改成 1.50 可以正常上传。<br>
**是不是很疑惑**：1.5 不等于 1.50 吗，1.5 不大于 1.46 吗。请往下看。


### 原因

**ERROR ITMS-90062**提示说 Info.plist 中 CFBundleShortVersionString 的值 1.5 版本不正确，请先看下苹果关于**[CFBundleShortVersionString][CFBundleShortVersionString_URL]**的解释。

 CFBundleShortVersionString  由三个分开的整数来代表版本号。如，1.3.2，每个整数都有不同的含义：
- `1` Major Revision
- `3` New Feature or Major Changes Revision
- `2` Maintenance Release Revision

版本号 1.5 与 1.46 高低，首先 Major Revision 对比都是1；再者 New Feature or Major Changes Revision，对比 5 < 46，确定 1.5 版本低于 1.46 版本。修改方法自然也比较简单。


### 延伸
1. 软件使用版本号主要是用来**追踪 BUG**、**标识产品**以及**方便沟通**。
2. Xcode 中 TAGETS > General > Idntity 中 Version 对应 Info.plist 中 **[CFBundleShortVersionString][CFBundleShortVersionString_URL]** 的值，Build 对应 Info.plist 中 **[CFBundleVersion][CFBundleVersion_URL]** 的值。Version 对外，用户可见；Build 对内，用户不可见<br> 
3. 建议 Build 和 Version 都使用三段式版本号，如 1.32.1 等。在开发新版本时 Build 可以使用一些后缀d（develop）、a（alpha）、b（beta）、fc（final candidate）等，关于 Build 和 Version,三段式版本号可以可看下苹果 **[CFBundleVersion][CFBundleVersion_URL]** 和 **[CFBundleShortVersionString][CFBundleShortVersionString_URL]** 文档。
4. 自动设置版本号
	- **[agvtool][agvtool man page]**，苹果提供的自动设置 Build 和 Version 工具，**[使用方法][agvtool使用方法]**。
    - 还可以将脚本添加到 Build Phase 中实现自动设置 Build 和 Version（TAGETS > Build Phase > 左上角 `+` 图标 > New Run Script Phase），**[使用方法][设置Xcode下Version Build版本号自增长脚本]**。

ps: 公司 APP 是自己在开发维护。如果 Build 时间为 2017.02.02 18:00，为了方便 CFBundleVersion 一般写成：201702021800。

### 参考

- [浅谈 iOS 版本号][浅谈 iOS 版本号]
- [设置Xcode下Version Build版本号自增长脚本][设置Xcode下Version Build版本号自增长脚本]
- [What values should I use for CFBundleVersion and CFBundleShortVersionString?][stackoverflow CFBundleVersion and CFBundleShortVersionString]
- [CFBundleShortVersionString][CFBundleShortVersionString_URL]
- [CFBundleVersion][CFBundleVersion_URL]
- [agvtool(8) Mac OS X Developer Tools Manual Page][agvtool man page]
- [Technical Q&A QA1827: Automating Version and Build Numbers Using agvtool][agvtool使用方法]


<!-- URLs -->

[stackoverflow CFBundleVersion and CFBundleShortVersionString]:http://stackoverflow.com/questions/19726988/what-values-should-i-use-for-cfbundleversion-and-cfbundleshortversionstring "What values should I use for CFBundleVersion and CFBundleShortVersionString?"
[CFBundleShortVersionString_URL]:https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/20001431-111349 "CFBundleShortVersionString"
[CFBundleVersion_URL]:https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/20001431-111349 "CFBundleVersion"
[agvtool man page]:https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/agvtool.1.html "agvtool(8) Mac OS X Developer Tools Manual Page"
[agvtool使用方法]:https://developer.apple.com/library/content/qa/qa1827/_index.html "Technical Q&A QA1827: Automating Version and Build Numbers Using agvtool"
[浅谈 iOS 版本号]:https://segmentfault.com/a/1190000002423661 "浅谈 iOS 版本号"
[设置Xcode下Version Build版本号自增长脚本]:http://www.jianshu.com/p/5c98023ac440 "设置Xcode下Version Build版本号自增长脚本"


