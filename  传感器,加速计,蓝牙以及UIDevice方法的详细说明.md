### 距离传感器:
`2016年04月05日 10:32:51发布于csdn`

网上关于距离传感器使用很简单,流程也少.下面用代码解释下怎么启动距离传感器并且进行监听。
```
// 1.开启距离传感器(注意: 默认情况距离传感器是关闭的)
// 只要开启之后, 就开始实时监听
[UIDevice currentDevice].proximityMonitoringEnabled = YES;


// 2.当监听到有物体靠近设备时系统会发出通知
[[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(proximityStateDidChange:) name:UIDeviceProximityStateDidChangeNotification object:nil];


// 当监听到有物体靠近设备时调用
- (void)proximityStateDidChange:(NSNotification *)note
{
  if( [UIDevice currentDevice].proximityState)//判断物体接近状态
  {
      NSLog(@"有物体靠近");
  }else{
      NSLog(@"物体离开");
  }
}
```

### 加速度传感器:
在iOS4以前使用UIAccelerometer来获取到设备的加速度,用法简单,但是在iOS5时已经过期.

但是从iOS4开始苹果提供了一个库,CoreMotion.framework来获取设备的加速度信息,功能更加强大
但是在iOS8已经完全不能使用
```
UIAccelerometer使用步骤:    //被废弃,简单写一点用法步骤

1.获得单例对象
UIAccelerometer *acc = [UIAccelerometer sharedAccelerometer];
2.设置代理
遵循<UIAccelerometerDelegate>协议
acc.delegate = self;
实现代理方法
- (void)accelerometer:(UIAccelerometer *)accelerometer didAccelerate:(UIAcceleration *)acceleration;
这个方法只要采集到数据就会调用

3.设置采样间隔
acc.updateInterval = 1 / 30;
```
CoreMotion使用步骤:

```
1.创建运动管理者对象
self.mgr = [[CMMotionManager alloc] init];
/*   CMMotionManager 属性
     isAccelerometerActive 是否正在采集
     accelerometerData 采集到得数据
     startAccelerometerUpdates  pull
     startAccelerometerUpdatesToQueue  push
     stopAccelerometerUpdates 停止采集
     accelerometerUpdateInterval 采样时间
*/

2.判断加速计是否可用(最好是要判断),断加速计可用则设置下面的数据
if (self.mgr.isAccelerometerAvailable) {}

3.设置采样间隔
self.mgr.accelerometerUpdateInterval = 0.1 ;/// 30.0;

4.开始采样
[self.mgr startAccelerometerUpdatesToQueue:[NSOperationQueuemainQueue]      
                               withHandler:^(CMAccelerometerData*accelerometerData, NSError *error) {
            // 这个block是采集到数据时就会调用
            if (error) return ;

            CMAcceleration acceleration = accelerometerData.acceleration;

            NSLog(@"x = %f y = %f z = %f", acceleration.x, acceleration.y, acceleration.z);

        }];
```

### UIDevice

```
typedef NS_ENUM(NSInteger, UIDeviceOrientation) //设备方向
{
   UIDeviceOrientationUnknown,
   UIDeviceOrientationPortrait,                   // 竖向，头向上
   UIDeviceOrientationPortraitUpsideDown,  // 竖向，头向下
   UIDeviceOrientationLandscapeLeft,         // 横向，头向左
   UIDeviceOrientationLandscapeRight,       // 横向，头向右
   UIDeviceOrientationFaceUp,                   // 平放，屏幕朝下
   UIDeviceOrientationFaceDown                // 平放，屏幕朝下
};

typedef NS_ENUM(NSInteger, UIDeviceBatteryState) //电池状态
{
   UIDeviceBatteryStateUnknown,
   UIDeviceBatteryStateUnplugged,   // 未充电
   UIDeviceBatteryStateCharging,     // 正在充电
   UIDeviceBatteryStateFull,             // 满电
};


typedef NS_ENUM(NSInteger, UIUserInterfaceIdiom) //用户界面类型
{
//iOS3.2以上有效
#if __IPHONE_3_2 <= __IPHONE_OS_VERSION_MAX_ALLOWED
   UIUserInterfaceIdiomPhone,           // iPhone 和 iPod touch 风格
   UIUserInterfaceIdiomPad,              // iPad 风格
#endif
};


#define UI_USER_INTERFACE_IDIOM() ([[UIDevice currentDevice] respondsToSelector:@selector(userInterfaceIdiom)] ? [[UIDevice currentDevice] userInterfaceIdiom] : UIUserInterfaceIdiomPhone)

#define UIDeviceOrientationIsPortrait(orientation)  ((orientation) == UIDeviceOrientationPortrait || (orientation) == UIDeviceOrientationPortraitUpsideDown)

#define UIDeviceOrientationIsLandscape(orientation) ((orientation) == UIDeviceOrientationLandscapeLeft || (orientation) == UIDeviceOrientationLandscapeRight)


NS_CLASS_AVAILABLE_IOS(2_0) @interface UIDevice : NSObject {

@private

   NSInteger _numDeviceOrientationObservers;

   float     _batteryLevel;

   struct {

unsigned int batteryMonitoringEnabled:1;

unsigned int proximityMonitoringEnabled:1;

unsigned int expectsFaceContactInLandscape:1;

       unsigned int orientation:3;

       unsigned int batteryState:2;

       unsigned int proximityState:1;

   } _deviceFlags;

}


+ (UIDevice *)currentDevice; // 获取当前设备


@property(nonatomic,readonly,retain) NSString    *name;                // e.g. "My iPhone"

@property(nonatomic,readonly,retain) NSString    *model;               // e.g. @"iPhone", @"iPod touch"

@property(nonatomic,readonly,retain) NSString    *localizedModel;    // localized version of model

@property(nonatomic,readonly,retain) NSString    *systemName;      // e.g. @"iOS"

@property(nonatomic,readonly,retain) NSString    *systemVersion;    // e.g. @"4.0"

@property(nonatomic,readonly) UIDeviceOrientation orientation;       // 除非正在生成设备方向的通知，否则返回UIDeviceOrientationUnknown 。


@property(nonatomic,readonly,retain) NSUUID      *identifierForVendor NS_AVAILABLE_IOS(6_0);      // 可用于唯一标识该设备，同一供应商不同应用具有相同的UUID 。


@property(nonatomic,readonly,getter=isGeneratingDeviceOrientationNotifications) BOOL generatesDeviceOrientationNotifications; //是否生成设备转向通知

- (void)beginGeneratingDeviceOrientationNotifications;

- (void)endGeneratingDeviceOrientationNotifications;


@property(nonatomic,getter=isBatteryMonitoringEnabled) BOOL batteryMonitoringEnabled NS_AVAILABLE_IOS(3_0);  // 是否启动电池监控，默认为NO

@property(nonatomic,readonly) UIDeviceBatteryState batteryState NS_AVAILABLE_IOS(3_0);  // 如果禁用电池监控，则电池状态为UIDeviceBatteryStateUnknown

@property(nonatomic,readonly) float batteryLevel NS_AVAILABLE_IOS(3_0);  //电量百分比， 0 .. 1.0。如果电池状态为UIDeviceBatteryStateUnknown，则百分比为-1.0


@property(nonatomic,getter=isProximityMonitoringEnabled) BOOL proximityMonitoringEnabled NS_AVAILABLE_IOS(3_0); // 是否启动接近监控（例如接电话时脸靠近屏幕），默认为NO

@property(nonatomic,readonly)  BOOL proximityState NS_AVAILABLE_IOS(3_0);  // 如果设备不具备接近感应器，则总是返回NO


@property(nonatomic,readonly,getter=isMultitaskingSupported) BOOL multitaskingSupported NS_AVAILABLE_IOS(4_0); // 是否支持多任务


@property(nonatomic,readonly) UIUserInterfaceIdiom userInterfaceIdiom NS_AVAILABLE_IOS(3_2); // 当前用户界面模式


- (void)playInputClick NS_AVAILABLE_IOS(4_2);  // 播放一个输入的声音

@end


@protocol UIInputViewAudioFeedback

@optional

@property (nonatomic, readonly) BOOL enableInputClicksWhenVisible; // 实现该方法，返回YES则自定义的视图能够播放输入的声音

@end


UIKIT_EXTERN NSString *const UIDeviceOrientationDidChangeNotification; // 屏幕方向变化通知

UIKIT_EXTERN NSString *const UIDeviceBatteryStateDidChangeNotification   NS_AVAILABLE_IOS(3_0); // 电池状态变化通知

UIKIT_EXTERN NSString *const UIDeviceBatteryLevelDidChangeNotification   NS_AVAILABLE_IOS(3_0); // 电池电量变化通知

UIKIT_EXTERN NSString *const UIDeviceProximityStateDidChangeNotification NS_AVAILABLE_IOS(3_0); // 接近状态变化通知
```
