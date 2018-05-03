UIDevice 系统信息详解
---

获取当前设备  
`UIDevice *dev = [UIDevice currentDevice];`

设备和系统基本信息
```
NSLog(@"设备名称：%@", dev.name);  
NSLog(@"设备类型：%@", dev.model);  
NSLog(@"本地化模式：%@", dev.localizedModel);
NSLog(@"系统名称:%@", dev.systemName);   
NSLog(@"系统版本：%@", dev.systemVersion);   
NSLog(@"设备朝向：%ld", dev.orientation);
NSLog(@"UUID:%@", dev.identifierForVendor.UUIDString);
```

设备类型判断
```
if (dev.userInterfaceIdiom == UIUserInterfaceIdiomPhone) {    
     NSLog(@"iPhone 设备");
}else if (dev.userInterfaceIdiom == UIUserInterfaceIdiomPad) {       
    NSLog(@"iPad 设备");
}else if (dev.userInterfaceIdiom == UIUserInterfaceIdiomTV) {     
    NSLog(@"Apple TV设备");
}else{     
    NSLog(@"未知设备！！");
}
```


### 电池相关信息

设置电池是否被监视   
`dev.batteryMonitoringEnabled = YES;`

判断当前电池状态
```
if (dev.batteryState == UIDeviceBatteryStateUnknown) {   
     NSLog(@"UnKnow");
}else if (dev.batteryState == UIDeviceBatteryStateUnplugged) {  
     NSLog(@"未充电");
}else if (dev.batteryState == UIDeviceBatteryStateCharging) {
     NSLog(@"正在充电，电量未满");
}else if (dev.batteryState == UIDeviceBatteryStateFull) {  
     NSLog(@"正在充电，电量已满");
}
```

当前电量等级 [0.0, 1.0]   
`NSLog(@"%f",dev.batteryLevel);`


### 红外线感应

```
//开启红外感应-- 用于检测手机是否靠近面部
dev.proximityMonitoringEnabled = YES;  
if (dev.proximityState == YES) {   
    NSLog(@"靠近面部");
} else {      
    NSLog(@"没有靠近");
}
```
