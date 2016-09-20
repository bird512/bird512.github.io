layout: post
title:  XCODE打包一个react native项目时的“Cyclic dependency”错误。 
comment: true
tags: [技术, react native, ios]
date: 2016-9-14 10:40:50
---

------
近日想把一个年初时就完的react native打包发布到app store. 这个项目最初是打算用客户的集团账号发布成in-house应用，但是客户的这个集团账号正好过期了，客户公司是一个上市大公司，跑流程跑了一个多月还是没完成续费。最后决定我们自己的发开账号发布成一个公共应用到app store。 之前已经在XCODE上用客户的集团账号通过Product->Archive顺利打出过一个发布包了，当我这次改掉team之后再Archive竟然出现了的错误。Clean，关掉重启都不起作用。

```
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS9.3.sdk/System/Library/Frameworks/QuartzCore.framework/Headers/BaiduMapAPI_Base.framework/Headers/BMKMapManager.h:10:9: Cyclic dependency in module 'UIKit': UIKit -> QuartzCore -> UIKit


/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS9.3.sdk/System/Library/Frameworks/UIKit.framework/Headers/UICollectionViewLayout.h:11:9: Could not build module 'QuartzCore'

/Users/xxx/gittangu/app/GuanLi2/node_modules/react-native/React/Base/RCTBridge.h:10:9: Could not build module 'UIKit'
```

Google "Cyclic dependency in module"，几经折腾还是不行。再仔细看下错误信息，晕IOS下面怎么会有BaiduMap的SDK包？删除之，再Archive，搞定！

```
cd /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS9.3.sdk/System/Library/Frameworks/QuartzCore.framework/Headers/
rm -rf BaiduMapAPI*
```

-----
不知道之前做了什么误操作把BaiduMap的SDK拷到ISO SDK下面去了？