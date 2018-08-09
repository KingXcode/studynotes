1. 方法在Mac和iOS上使用的最低版本
    - `@avalibale`同步判断当前iOS系统是否满足需求。    *==这个方法可能用到==*
       
        ```
        if (@available(iOS 11, *)) { // >= 11
            NSLog(@"XXX1");
        } else if (@available(iOS 10, *)) { //>= 10
            NSLog(@"XXX2");
        } else { // < 10
            NSLog(@"XXX3");
        }  
        ```
    - 如果是对方法的声明 添加`API_AVAILABLE(macos(10.5),ios(11))` ，就不用再方法里面做系统版本判断了，错用会有警告提示。

    
2. 方法在哪个版本弃用了
    - NS_DEPRECATED_IOS(2_0,6_0) 前面2_0代表iOS系统，表示这个方法被引用时的iOS版本，后面6_0代表iOS系统，表示这个方法被弃用时的iOS版本。被弃用并不是指这个方法就不存在，它会告诉用户去使用新方法

    ```objc
    #define HTDeprecated(instead) NS_DEPRECATED(2_0, 2_0, 2_0, 2_0, instead)
    ```

3. 方法被弃用了   *==这个方法可能用到==*
    - DEPRECATED_ATTRIBUTE 它会告诉编译器该方法被弃用了

    - DEPRECATED_MSG_ATTRIBUTE(s) 它会告诉编译器该方法被弃用了，后面s 代表提示

    - __deprecated_msg(_msg) 它会告诉编译器该方法被弃用了，后面_msg代表提示

4. 判断当前设备是模拟器还是真机
    
    ```
    #if TARGET_IPHONE_SIMULATOR
    // 模拟器
    #elif TARGET_OS_IPHONE
    // 真机
    #endif
    ```
    
5. 判断当前设备系统

    ```
    #if __IPHONE_OS_VERSION_MAX_ALLOWED >= __IPHONE_8_0
        // 这里写设备系统大于8.0 以上的代码
    #else
        // 这里写设备系统小于8.0以上的代码
    #endif
    
    
    #if __IPHONE_OS_VERSION_MIN_REQUIRED <= __IPHONE_7_0
        // 这里写设备系统小于7.0以上的代码
    #else
        // 这里写设备系统大于7.0以上的代码
    #endif
    ```

6. 日志输出

    ```
    #ifdef DEBUG
    #define HTLog(...) NSLog(__VA_ARGS__)
    #else
    #define HTLog(...)
    #endif
    //此示例的DEBUG不唯一，如果定义了多个target可以去自定义
    ```

7. 指定初始化方法`NS_DESIGNATED_INITIALIZER`

    即接收参数最多的那个初始化方法，其他初始化方法调用它即可，这样设计的目的是为了保证所有初始化方法都正确地初始化实例变量。
    在方法后面加上`NS_DESIGNATED_INITIALIZER`宏即可。这样，当你子类化这个类时，在子类的初始化方法里如果没有正确地调用父类的designated initializer，编译器就会给出警告。
    实例代码：

    ```objc 
    @interface WKWebView : UIView
    - (instancetype)initWithFrame:(CGRect)frame configuration:(WKWebViewConfiguration *)configuration NS_DESIGNATED_INITIALIZER;
    - (nullable instancetype)initWithCoder:(NSCoder *)coder NS_DESIGNATED_INITIALIZER;
    @end
    ```


