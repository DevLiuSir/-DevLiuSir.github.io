---
layout: post
title:  iOS基本设备信息查询
date:   2017-09-21 22:21:49
categories: 能工巧匠集
tags: 能工巧匠集
---



开发中总会遇到很多需要查询设备及App信息的情况,有时候也是为了更好的用户体验或者为了bug跟踪，可能会需要获取用户的应用信息、系统信息、设备信息。这些信息的获取可以根据不同的设备或者App、系统版本来提供不同的功能或更好的用户体验，或者让开发者能更好的分析用户的问题原因。

因此我查询了很多资料,整理并更新记录起来,为了方便自己日后查询,同时分享给大家,不多说,撸代码吧.

## (一)设备及App信息查询

1.获取设备名称

OC代码

```objectivec
NSString *deviceName = [[UIDevice currentDevice] name];
```

Swift代码

```swift
let deviceName = UIDevice.currentDevice().name
```

2.获取系统版本号

OC代码

```objectivec
NSString *sysVersion = [[UIDevice currentDevice] systemVersion];
```

Swift代码

```swift
let sysVersion =     UIDevice.currentDevice().systemVersion
```

3.获取设备唯一标识符

OC代码

```objectivec
NSString *deviceUUID = [[[UIDevice currentDevice] identifierForVendor] ;
```

Swift代码

```swift
let deviceUUID = UIDevice.currentDevice().identifierForVendor?.UUIDString
```

4.获取设备型号

OC代码

```objectivec
NSString *deviceModel = [[UIDevice currentDevice] model];
```

Swift代码

```swift
let deviceModel = UIDevice.currentDevice().model
```

5.获取App相关的信息

OC代码

```objectivec
 NSDictionary *infoDic = [[NSBundle mainBundle] infoDictionary];
 // 获取App的版本号
 NSString *appVersion = [infoDic objectForKey:@"CFBundleShortVersionString"];
 // 获取App的build版本
 NSString *appBuildVersion = [infoDic objectForKey:@"CFBundleVersion"];
 // 获取App的名称
 NSString *appName = [infoDic objectForKey:@"CFBundleDisplayName"];
```
 
Swift代码

```swift
let infoDic = NSBundle.mainBundle().infoDictionary
 // 获取App的版本号
 let appVersion = infoDic?["CFBundleShortVersionString"]
 // 获取App的build版本
 let appBuildVersion = infoDic?["CFBundleVersion"]
 // 获取App的名称
 let appName = infoDic?["CFBundleDisplayName"]
```
 
## (二)iOS获取具体设备型号

OC代码获取iOS设备型号：

```objectivec
// 需要#import + (NSString*)deviceModelName
{
    struct utsname systemInfo;
    uname(&systemInfo);
    NSString *deviceModel = [NSString stringWithCString:systemInfo.machine encoding:NSUTF8StringEncoding];
    //iPhone 系列
    if ([deviceModel isEqualToString:@"iPhone1,1"])    return @"iPhone 1G";
    if ([deviceModel isEqualToString:@"iPhone1,2"])    return @"iPhone 3G";
    if ([deviceModel isEqualToString:@"iPhone2,1"])    return @"iPhone 3GS";
    if ([deviceModel isEqualToString:@"iPhone3,1"])    return @"iPhone 4";
    if ([deviceModel isEqualToString:@"iPhone3,2"])    return @"Verizon iPhone 4";
    if ([deviceModel isEqualToString:@"iPhone4,1"])    return @"iPhone 4S";
    if ([deviceModel isEqualToString:@"iPhone5,1"])    return @"iPhone 5";
    if ([deviceModel isEqualToString:@"iPhone5,2"])    return @"iPhone 5";
    if ([deviceModel isEqualToString:@"iPhone5,3"])    return @"iPhone 5C";
    if ([deviceModel isEqualToString:@"iPhone5,4"])    return @"iPhone 5C";
    if ([deviceModel isEqualToString:@"iPhone6,1"])    return @"iPhone 5S";
    if ([deviceModel isEqualToString:@"iPhone6,2"])    return @"iPhone 5S";
    if ([deviceModel isEqualToString:@"iPhone7,1"])    return @"iPhone 6 Plus";
    if ([deviceModel isEqualToString:@"iPhone7,2"])    return @"iPhone 6";
    if ([deviceModel isEqualToString:@"iPhone8,1"])    return @"iPhone 6s";
    if ([deviceModel isEqualToString:@"iPhone8,2"])    return @"iPhone 6s Plus";
    if ([deviceModel isEqualToString:@"iPhone9,1"])    return @"iPhone 7 (CDMA)";
    if ([deviceModel isEqualToString:@"iPhone9,3"])    return @"iPhone 7 (GSM)";
    if ([deviceModel isEqualToString:@"iPhone9,2"])    return @"iPhone 7 Plus (CDMA)";
    if ([deviceModel isEqualToString:@"iPhone9,4"])    return @"iPhone 7 Plus (GSM)";
    //iPod 系列
    if ([deviceModel isEqualToString:@"iPod1,1"])      return @"iPod Touch 1G";
    if ([deviceModel isEqualToString:@"iPod2,1"])      return @"iPod Touch 2G";
    if ([deviceModel isEqualToString:@"iPod3,1"])      return @"iPod Touch 3G";
    if ([deviceModel isEqualToString:@"iPod4,1"])      return @"iPod Touch 4G";
    if ([deviceModel isEqualToString:@"iPod5,1"])      return @"iPod Touch 5G";
    //iPad 系列
    if ([deviceModel isEqualToString:@"iPad1,1"])      return @"iPad";
    if ([deviceModel isEqualToString:@"iPad2,1"])      return @"iPad 2 (WiFi)";
    if ([deviceModel isEqualToString:@"iPad2,2"])      return @"iPad 2 (GSM)";
    if ([deviceModel isEqualToString:@"iPad2,3"])      return @"iPad 2 (CDMA)";
    if ([deviceModel isEqualToString:@"iPad2,4"])      return @"iPad 2 (32nm)";
    if ([deviceModel isEqualToString:@"iPad2,5"])      return @"iPad mini (WiFi)";
    if ([deviceModel isEqualToString:@"iPad2,6"])      return @"iPad mini (GSM)";
    if ([deviceModel isEqualToString:@"iPad2,7"])      return @"iPad mini (CDMA)";
    if ([deviceModel isEqualToString:@"iPad3,1"])      return @"iPad 3(WiFi)";
    if ([deviceModel isEqualToString:@"iPad3,2"])      return @"iPad 3(CDMA)";
    if ([deviceModel isEqualToString:@"iPad3,3"])      return @"iPad 3(4G)";
    if ([deviceModel isEqualToString:@"iPad3,4"])      return @"iPad 4 (WiFi)";
    if ([deviceModel isEqualToString:@"iPad3,5"])      return @"iPad 4 (4G)";
    if ([deviceModel isEqualToString:@"iPad3,6"])      return @"iPad 4 (CDMA)";
    if ([deviceModel isEqualToString:@"iPad4,1"])      return @"iPad Air";
    if ([deviceModel isEqualToString:@"iPad4,2"])      return @"iPad Air";
    if ([deviceModel isEqualToString:@"iPad4,3"])      return @"iPad Air";
    if ([deviceModel isEqualToString:@"iPad5,3"])      return @"iPad Air 2";
    if ([deviceModel isEqualToString:@"iPad5,4"])      return @"iPad Air 2";
    if ([deviceModel isEqualToString:@"i386"])         return @"Simulator";
    if ([deviceModel isEqualToString:@"x86_64"])       return @"Simulator";
    if ([deviceModel isEqualToString:@"iPad4,4"]
        ||[deviceModel isEqualToString:@"iPad4,5"]
        ||[deviceModel isEqualToString:@"iPad4,6"])      return @"iPad mini 2";
    if ([deviceModel isEqualToString:@"iPad4,7"]
        ||[deviceModel isEqualToString:@"iPad4,8"]
        ||[deviceModel isEqualToString:@"iPad4,9"])      return @"iPad mini 3";
    return deviceModel;
}
```

记得一定要 **#import** 啊

Swift代码获取iOS设备型号：

```swift
//MARK: - UIDevice延展
public extension UIDevice {
    var modelName: String {
        var systemInfo = utsname()
        uname(&systemInfo)
        let machineMirror = Mirror(reflecting: systemInfo.machine)
        let identifier = machineMirror.children.reduce("") { identifier, element in
            guard let value = element.value as? Int8 where value != 0 else { return identifier }
            return identifier + String(UnicodeScalar(UInt8(value)))
        }
        switch identifier {
        case "iPod5,1":                                 return "iPod Touch 5"
        case "iPod7,1":                                 return "iPod Touch 6"
        case "iPhone3,1", "iPhone3,2", "iPhone3,3":     return "iPhone 4"
        case "iPhone4,1":                               return "iPhone 4s"
        case "iPhone5,1", "iPhone5,2":                  return "iPhone 5"
        case "iPhone5,3", "iPhone5,4":                  return "iPhone 5c"
        case "iPhone6,1", "iPhone6,2":                  return "iPhone 5s"
        case "iPhone7,2":                               return "iPhone 6"
        case "iPhone7,1":                               return "iPhone 6 Plus"
        case "iPhone8,1":                               return "iPhone 6s"
        case "iPhone8,2":                               return "iPhone 6s Plus"
        case "iPhone9,1":                               return "iPhone 7 (CDMA)"
        case "iPhone9,3":                               return "iPhone 7 (GSM)"
        case "iPhone9,2":                               return "iPhone 7 Plus (CDMA)"
        case "iPhone9,4":                               return "iPhone 7 Plus (GSM)"
        case "iPad2,1", "iPad2,2", "iPad2,3", "iPad2,4":return "iPad 2"
        case "iPad3,1", "iPad3,2", "iPad3,3":           return "iPad 3"
        case "iPad3,4", "iPad3,5", "iPad3,6":           return "iPad 4"
        case "iPad4,1", "iPad4,2", "iPad4,3":           return "iPad Air"
        case "iPad5,3", "iPad5,4":                      return "iPad Air 2"
        case "iPad2,5", "iPad2,6", "iPad2,7":           return "iPad Mini"
        case "iPad4,4", "iPad4,5", "iPad4,6":           return "iPad Mini 2"
        case "iPad4,7", "iPad4,8", "iPad4,9":           return "iPad Mini 3"
        case "iPad5,1", "iPad5,2":                      return "iPad Mini 4"
        case "iPad6,7", "iPad6,8":                      return "iPad Pro"
        case "AppleTV5,3":                              return "Apple TV"
        case "i386", "x86_64":                          return "Simulator"
        default:                                        return identifier
        }
    }
}
```

## (三)其他设备信息查询(不经常用)

```objectivec
- (void)deviceInfo {
    UIDevice *dev = [UIDevice currentDevice];
     
    NSLog(@"是否支持多任务：%@", dev.multitaskingSupported?@"是":@"否");
    NSLog(@"设备名字：%@", dev.name);
    NSLog(@"系统名字：%@", dev.systemName);
    NSLog(@"设备model：%@", dev.model);
    NSLog(@"设备本地化model：%@", dev.localizedModel);
    NSLog(@"用户界面类型：%ld", (long)dev.userInterfaceIdiom);
    NSLog(@"设备厂商标识：%@", dev.identifierForVendor);
    NSLog(@"设备方向：%ld", (long)dev.orientation);
    NSLog(@"是否可以生成设备方向通知：%@", dev.generatesDeviceOrientationNotifications?@"是":@"否");
    NSLog(@"设备电量：%f", dev.batteryLevel);
    NSLog(@"电量监测是否启用：%@", dev.batteryMonitoringEnabled?@"是":@"否");
    NSLog(@"设备电量状态：%ld", (long)dev.batteryState);
    NSLog(@"距离感应器是否可以使用：%@", dev.proximityMonitoringEnabled?@"是":@"否");
    NSLog(@"距离感应器是否打开：%@", dev.proximityState?@"是":@"否");
}
```
