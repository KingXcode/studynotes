Quartz2D学习记录
---------------

##### Quartz2D 概述及作用

Quartz2D 的 API 是纯 C 语言的，Quartz2D 的 API 来自于 Core Graphics 框架。

数据类型和函数基本都以 CG 作为前缀,如`CGContextRef`,`CGPathRef`,`CGContextStrokePath(ctx)`...

```
Quartz 2D 是一个二维绘图引擎，同时支持 iOS 和 Mac 系统。
Quartz 2D 能完成的工作:
绘制图形 : 线条\三角形\矩形\圆\弧等;
绘制文字;
绘制\生成图片(图像);
读取\生成 PDF;
截图\裁剪图片;
自定义 UI 控件;
```
---
```
这里记录一点关于图形上下文Context的解释,这个东西我曾很长一段时间不是很理解其存在的意义.

图形上下文(Graphics Context):是一个 CGContextRef 类型的数据。
图形上下文的作用:
(1)保存绘图信息、绘图状态
(2)决定绘制的输出目标(绘制到什么地方去?)
  (输出目标可以是 PDF 文件、Bitmap 或者显示器的窗口上)
  (输出目标可以是 PDF 文件、Bitmap 或者显示器的窗口上)

相同的一套绘图序列，指定不同的 Graphics Context，就可将相同的图像绘制到不同的目标上。
```



---



自定义 view 的步骤:
1. 新建一个类，继承自 `UIView`
2. 实现`- (void)drawRect:(CGRect)rect`方法，然后在这个方法中\
    (a) 取得跟当前 `view` 相关联的图形上下文\
    例如:\
    `CGContextRef ctx = UIGraphicsGetCurrentContext();`

    (b) 绘制相应的图形内容\
    例如:画 1/4 圆
    ```
    CGContextMoveToPoint(ctx, 100, 100);
    CGContextAddLineToPoint(ctx, 100, 150);
    CGContextAddArc(ctx, 100, 100, 50, -M_PI_2, M_PI, 1);
     CGContextClosePath(ctx);
    [[UIColor redColor] set];
    ```
3. 利用图形上下文将绘制的所有内容渲染显示到 `view` 上面 \
    例如:`CGContextFillPath(ctx);`




---

#### drawRect?
为什么要实现`drawRect:`方法才能绘图到 `view` 上?\
因为在`drawRect:`方法中才能取得跟 `view` 相关联的图形上下文。\
`drawRect:`方法在什么时候被调用?

(1)当 `view` 第一次显示到屏幕上时(被加到 UIWindow 上显示出来)。 \
(2)调用 `view` 的`setNeedsDisplay` 或者`setNeedsDisplayInRect:`时。

`setNeedsDisplay 常被调用来刷新 View 界面。`

---




Quartz2D 绘图的代码步骤

第一步:获得图形上下文:\
`CGContextRef ctx = UIGraphicsGetCurrentContext(); `

第二步:拼接路径(下面代码是搞一条线段):\
`CGContextMoveToPoint(ctx, 10, 10); `\
`CGContextAddLineToPoint(ctx, 100, 100); `

第三步:绘制路径:\
`CGContextStrokePath(ctx); // CGContextFillPath(ctx);`

```
在 drawRect:方法中取得上下文后，就可以绘制东西到 view 上。
View 内部有个 layer(图层)属性，drawRect: 方法中取得的是一个 Layer Graphics Context，
因此，绘制的东西其实是绘制到 view 的 layer 上去了。
View 之所 以能显示东西，完全是因为它内部的 layer。
```

---




```
常用拼接路径函数
• 新建一个起点
void CGContextMoveToPoint(CGContextRef c, CGFloat x, CGFloat y)
• 添加新的线段到某个点
void CGContextAddLineToPoint(CGContextRef c, CGFloat x, CGFloat y)
• 添加一个矩形
void CGContextAddRect(CGContextRef c, CGRect rect) • 添加一个椭圆
void CGContextAddEllipseInRect(CGContextRef context, CGRect rect)
• 添加一个圆弧
void CGContextAddArc(CGContextRef c, CGFloat x, CGFloat y, CGFloat radius, CGFloat startAngle, CGFloat endAngle, int clockwise)
```




```
常用绘制路径函数
• Mode 参数决定绘制的模式
void CGContextDrawPath(CGContextRef c, CGPathDrawingMode mode)
• 绘制空心路径
void CGContextStrokePath(CGContextRef c)
• 绘制实心路径
void CGContextFillPath(CGContextRef c)

提示:一般以 CGContextDraw、CGContextStroke、CGContextFill 开头的函数，都是用来绘制路径的

其他常用函数:

设置线段宽度:       
CGContextSetLineWidth(ctx, 10);

设置线段头尾部的样式:
CGContextSetLineCap(ctx, kCGLineCapRound);

设置线段转折点的样式:
CGContextSetLineJoin(ctx, kCGLineJoinRound);

设置颜色:
CGContextSetRGBStrokeColor(ctx, 1, 0, 0, 1);
```



```
矩阵操作
利用矩阵操作，能让绘制到上下文中的所有路径一起发生变化:
缩放:
void CGContextScaleCTM(CGContextRef c, CGFloat sx, CGFloat sy)
旋转:
void CGContextRotateCTM(CGContextRef c, CGFloat angle)
平移:
void CGContextTranslateCTM(CGContextRef c, CGFloat tx, CGFloat ty)
```
---

### Quartz2D 的内存管理
关于内存管理，有以下原则:

(1)使用含有`“Create”`或`“Copy”`的函数创建的对象，使用完后必须释放，否则将导致内存泄露。 \
(2)使用不含有`“Create”`或`“Copy”`的函数获取的对象，则不需要释放\
(3)如果 `retain` 了一个对象，不再使用时，需要将其 `release` 掉。\
(4)可以使用 `Quartz 2D` 的函数来指定 `retain` 和 `release` 一个对象。例如，如果创建了一个 `CGColorSpace` 对象， 则使用函数 `CGColorSpaceRetain` 和 `CGColorSpaceRelease` 来 `retain` 和 `release` 对象。\
(5)也可以使用 `Core Foundation` 的 `CFRetain` 和 `CFRelease`。注意不能传递 `NULL` 值给这些函数。

---

### 图片水印示例


实现方式:\
利用 Quartz2D，将水印(文字、LOGO)画到图片的右下角


```
UIImage *bgImage = [UIImage imageNamed:@"scene"];

// 上下文 : 基于位图(bitmap) , 所有的东西需要绘制到一张新的图片上去
// 1.创建一个基于位图的上下文(开启一个基于位图的上下文)
// size : 新图片的尺寸
//opaque:YES: 不透明, NO: 透明
// 这行代码过后.就相当于创建一张新的 bitmap,也就是新的 UIImage 对象
UIGraphicsBeginImageContextWithOptions(bgImage.size, NO, 0.0);

// 2.画背景
//特别注意的是使用 OC 自带的画图方法不用传上下文，系统会自动检测并传入上下文
[bgImage drawInRect:CGRectMake(0, 0, bgImage.size.width, bgImage.size.height)];

// 3.画右下角的水印
UIImage *waterImage = [UIImage imageNamed:@"logo"]; CGFloat scale = 0.2;
CGFloat margin = 5;
CGFloat waterW = waterImage.size.width * scale;
CGFloat waterH = waterImage.size.height * scale;
CGFloat waterX = bgImage.size.width - waterW - margin;
CGFloat waterY = bgImage.size.height - waterH - margin;
[waterImage drawInRect:CGRectMake(waterX, waterY, waterW, waterH)];

// 4.从上下文中取得制作完毕的 UIImage 对象
UIImage *newImage = UIGraphicsGetImageFromCurrentImageContext();

// 5.结束上下文
UIGraphicsEndImageContext();

// 6.显示到 UIImageView
self.iconView.image = newImage;

// 7.将 image 对象压缩为 PNG 格式的二进制数据
NSData *data = UIImagePNGRepresentation(newImage); //UIImageJPEGRepresentation(<#UIImage *image#>, <#CGFloat compressionQuality#>)

// 8.写入文件
NSString *path =
[[NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) lastObject]
stringByAppendingPathComponent: @"new.png"];
[data writeToFile:path atomically:YES];
```





### 图片裁剪示例
核心代码:\
`void CGContextClip(CGContextRef c)`\
将当前上下所绘制的路径裁剪出来(超出这个裁剪区域的都不能显示)

- 示例1
```
CGContextRef ctx = UIGraphicsGetCurrentContext();
// 画圆
CGContextAddEllipseInRect(ctx, CGRectMake(100, 100, 50, 50)); // 裁剪
CGContextClip(ctx);
CGContextFillPath(ctx);
// 显示图片
UIImage *image = [UIImage imageNamed:@"me"];
[image drawAtPoint:CGPointMake(100, 100)];
```
- 示例2
```
(产生图像圆截图，效果和上面图片效果一样但在图像周围加个圆边框，并保存到本地):

// 1.加载原图
UIImage *oldImage = [UIImage imageNamed:@"me"];

// 2.开启上下文
CGFloat borderW = 2; // 圆环的宽度
CGFloat imageW = oldImage.size.width + 2 * borderW;
CGFloat imageH = oldImage.size.height + 2 * borderW;
CGSize imageSize = CGSizeMake(imageW, imageH); UIGraphicsBeginImageContextWithOptions(imageSize, NO, 0.0);

// 3.取得当前的上下文
CGContextRef ctx = UIGraphicsGetCurrentContext();

// 4.画边框(大圆)
[[UIColor whiteColor] set];
CGFloat bigRadius = imageW * 0.5; // 大圆半径 CGFloatcenterX=bigRadius;// 圆心
CGFloat centerY = bigRadius;
CGContextAddArc(ctx, centerX, centerY, bigRadius, 0, M_PI * 2, 0);
CGContextFillPath(ctx); // 画圆

// 5.小圆
CGFloat smallRadius = bigRadius - borderW;
CGContextAddArc(ctx, centerX, centerY, smallRadius, 0, M_PI * 2, 0); // 裁剪(后面画的东西才会受裁剪的影响)
CGContextClip(ctx);

// 6.画图
[oldImage drawInRect:CGRectMake(borderW, borderW, oldImage.size.width, oldImage.size.height)];

// 7.取图
UIImage *newImage = UIGraphicsGetImageFromCurrentImageContext();

// 8.结束上下文
UIGraphicsEndImageContext();

// 9.显示图片
self.iconView.image = newImage;

// 10.写出文件
NSData *data = UIImagePNGRepresentation(newImage);
NSString *path = [[NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) lastObject] stringByAppendingPathComponent:@"new.png"];
[data writeToFile:path atomically:YES];
```

### 屏幕截图示例
核心代码:
`- (void)renderInContext:(CGContextRef)ctx;`\
调用某个 `view` 的 `layer` 的 `renderInContext:`方法即可

示例代码:

```
// 1.开启上下文
UIGraphicsBeginImageContextWithOptions(view.frame.size, NO, 0.0);

// 2.将控制器 view 的 layer 渲染到上下文
[view.layer renderInContext:UIGraphicsGetCurrentContext()];

// 3.取出图片
UIImage *newImage = UIGraphicsGetImageFromCurrentImageContext();

// 4.写文件
NSData *data = UIImagePNGRepresentation(newImage);
NSString *path = [[NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) lastObject] stringByAppendingPathComponent:@"new.png"];
[data writeToFile:path atomically:YES];

// 5.结束上下文
UIGraphicsEndImageContext();
```

### OC 中自带画图方法
```
// 1.获得当前的触摸点
UITouch *touch = [touches anyObject];
CGPoint startPos = [touch locationInView:touch.view];

// 2.创建一个新的路径
UIBezierPath *currenPath = [UIBezierPath bezierPath];//也可用 alloc init 方法创建 currenPath.lineCapStyle = kCGLineCapRound;
currenPath.lineJoinStyle = kCGLineJoinRound;

// 3.设置起点
[currenPath moveToPoint:startPos];

//4.连线
[currentPath addLineToPoint:endPos];

```

判断一个点是否在一个矩形框内:\
CGRectContainsPoint(rect,point);//判断 point 这个点是否在 rect 这个矩形框内
