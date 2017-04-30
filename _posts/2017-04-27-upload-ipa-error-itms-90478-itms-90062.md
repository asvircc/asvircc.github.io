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
**解决**：方法比较简单，把 `TAGETS` > `General` > `Idntity` 中 `Version` : 1.5 修改成 1.50 可以正常上传。原因往下看。<br>
**是不是很疑惑**：1.5 不等于 1.50 吗，1.5 不大于 1.46 吗。


### 原因

苹果有关于 **[CFBundleVersion][CFBundleVersion_URL]** 和 **[CFBundleShortVersionString][CFBundleShortVersionString_URL]** 的详细解释。

`CFBundleShortVersionString` 由三个分开的整数来代表版本号。如，1.3.2，每个整数都有不同的含义：
- `1` Major Revision
- `3` New Feature or Major Changes Revision
- `2` Maintenance Release Revision

版本号高低对比是按照每个对应的位置来进行对比的。所以 1.5 与 1.46 版本号高低主要是通过 new feature or major changes 版本号对比，很明显 5 < 46。

`CFBundleVersion` 与 `CFBundleShortVersionString` 类似。

### 延伸
1. `CFBundleShortVersionString` 和 `CFBundleVersion` 没有直接的关系。<br>
2. 通过代码获取到版本号，不要直接拿字符串对比，要按照每个版本位单独对比。<br>
3. 建议 APP 版本号按照 `development`, `alpha`, `beta`, and `final candidate`, by `d`, `a`, `b`, and `fc`方式处理。<br>

PS: 公司 APP 是自己在开发维护。如果 Build 时间为 2017.02.02 18:00，所以自己为了方便 `CFBundleVersion` 一般写成：201702021800.

### 参考

- [What values should I use for CFBundleVersion and CFBundleShortVersionString?][stackoverflow CFBundleVersion and CFBundleShortVersionString]
- [CFBundleShortVersionString][CFBundleShortVersionString_URL]
- [CFBundleVersion][CFBundleVersion_URL]

<!-- URLs -->

[stackoverflow CFBundleVersion and CFBundleShortVersionString]:http://stackoverflow.com/questions/19726988/what-values-should-i-use-for-cfbundleversion-and-cfbundleshortversionstring "What values should I use for CFBundleVersion and CFBundleShortVersionString?"

[CFBundleShortVersionString_URL]:https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/20001431-111349 "CFBundleShortVersionString"

[CFBundleVersion_URL]:https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/20001431-111349 "CFBundleVersion"
