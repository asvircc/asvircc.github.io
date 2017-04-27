---
layout: post
title:  关于 APP 的版本
date:   2017-04-27 23:10
categories: iOS
author: asvircc
---

>#### 问题

最近助理反馈一个问题，APP 无法连接机器。具体 APP 名字都快没印象了。查看了 APP 更新信息，15年8月最后一次更新。问题还好，很快搞定了。打包上传的时候出错了，给了下面的提示信息

ERROR **ITMS-90478**: "Invalid Version. The build with the version “1.46” can’t be imported because a later version has been closed for new build submissions. Choose a different version number."

ERROR **ITMS-90062**: "This bundle is invalid. The value for key CFBundleShortVersionString [1.5] in the Info.plist file must contain a higher version than that of the previously approved version [1.46]."

**解决方法**：这个APP解决方法比较简单，把 TAGETS > General > Idntity 中 `Version` 和 `Build`: 1.5 修改成 1.50 可以正常上传。原因往下看。

>#### CFBundleShortVersionString

**CFBundleShortVersionString** (String - iOS, macOS) specifies the release version number of the bundle, which identifies a released iteration of the app.

The release version number is a string composed of three period-separated integers. The first integer represents major revision to the app, such as a revision that implements new features or major changes. The second integer denotes a revision that implements less prominent features. The third integer represents a maintenance release revision.

The value for this key differs from the value for CFBundleVersion, which identifies an iteration (released or unreleased) of the app.

This key can be localized by including it in your InfoPlist.strings files.

See also `NSHumanReadableCopyright`.

>#### CFBundleVersion

**CFBundleVersion** (String - iOS, macOS) specifies the build version number of the bundle, which identifies an iteration (released or unreleased) of the bundle.

The build version number should be a string comprised of three non-negative, period-separated integers with the first integer being greater than zero—for example, 3.1.2. The string should only contain numeric (0-9) and period (.) characters. Leading zeros are truncated from each integer and will be ignored (that is, 1.02.3 is equivalent to 1.2.3). The meaning of each element is as follows: 
- The first number represents the most recent major release and is limited to a maximum length of four digits.
- The second number represents the most recent significant revision and is limited to a maximum length of two digits.
- The third number represents the most recent minor bug fix and is limited to a maximum length of two digits.
- If the value of the third number is 0, you can omit it and the second period.

While developing a new version of your app, you can include a suffix after the number that is being updated; for example `3.1.3a1`. The character in the suffix represents the stage of development for the new version. For example, you can represent `development`, `alpha`, `beta`, and `final candidate`, by `d`, `a`, `b`, and `fc`. The final number in the suffix is the build version, which cannot be 0 and cannot exceed 255. When you release the new version of your app, remove the suffix.

>#### 原因

暂时参考苹果关于`CFBundleShortVersionString`和`CFBundleVersion`解释，有时间解释一下。


>#### 参考

- [What values should I use for CFBundleVersion and CFBundleShortVersionString?](http://stackoverflow.com/questions/19726988/what-values-should-i-use-for-cfbundleversion-and-cfbundleshortversionstring)

