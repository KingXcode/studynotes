###### 本来想研究下UIAlertController怎么能自定义界面，结果苹果没有提供相应的方法，没有办法进行自定制，结果从网上找出的一些方法都是都不好用，于是想到用私有的属性设置，于是使用runtime打印出alertController和alertAction的属性，然后用利用kvc进行私有属性的设置，发现也只能很简单的进行简单的定制。比如 action设置左边的图片，右边设置打钩的状态，这些私有属性都贴在文章后面，下面简单写点alertController 的用法。

```
+ (instancetype)alertControllerWithTitle:(nullable NSString *)title
                                 message:(nullable NSString *)message
                          preferredStyle:(UIAlertControllerStyle)preferredStyle;
```
上述方法为创建方法。

分别传title（标题），massage（详细信息），preferredStyle（弹出风格）

每个按钮都为自己创建UIAlertAction实例，以下是创建方法。


```
+ (instancetype)actionWithTitle:(nullable NSString *)title
                          style:(UIAlertActionStyle)style
                        handler:(void (^ __nullable)(UIAlertAction *action))handler;
title（按钮标题），style（风格定义如下），handler（按钮的点击事件，是一个代码块）

typedef NS_ENUM(NSInteger, UIAlertActionStyle) {
   UIAlertActionStyleDefault = 0,
   UIAlertActionStyleCancel,
   UIAlertActionStyleDestructive
} NS_ENUM_AVAILABLE_IOS(8_0);
```

创建好action之后添加到alertController中，以下方法添加：
```
- (void)addAction:(UIAlertAction *)action;
```

如果想在alertController中添加输入框，用以下方法：
```
- (void)addTextFieldWithConfigurationHandler:(void (^ __nullable)(UITextField *textField))configurationHandler;

configurationHandler是一个代码块，用于textField的初始化工作。
```

以下属性可以获得alertController中所有输入框实例，以数组的形式返回：

`@property (nullable, nonatomic, readonly) NSArray<UITextField *> *textFields;`




以下说的才是重点：

给每个action设置图片，系统没有提供方法，但是可以用KVC的方式进行设置

```
UIImage *image = [UIImage imageNamed:@"Untitled.jpg"];
image = [image imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
[action setValue:image forKey:@"_image"];
_image是UIAction中的一个私有属性，是对action进行图片设置的。
```

其实UIAction还有很多的私有属性，如下：
```
_title    设置文字的
_titleTextAlignment    设置文字的对齐方式
_enabled  设置是否可用
_checked  右方打钩
_isPreferred
_imageTintColor
_titleTextColor  设置文字颜色
_style
_handler
_simpleHandler
_image   设置图片
_shouldDismissHandler
__descriptiveText
_contentViewController
_keyCommandInput
_keyCommandModifierFlags
__representer
__alertController
```

以下是UIAlertViewController的私有属性：

```
_message
_attributedTitle
_attributedMessage
_attributedDetailMessage
_actionDelimiterIndices
_linkedAlertControllers
_cancelAction
_actionToKeyCommandsDictionary
_keyCommandToActionMapTable
_resolvedStyle
_preferredStyle
_styleProvider
_contentViewController
_textFieldViewController
_backButtonDismissGestureRecognizer
_selectGestureRecognizer
_ownedTransitioningDelegate
_shouldInformViewOfAddedContentViewController
_isInSupportedInterfaceOrientations
_shouldEnsureContentControllerViewIsVisibleOnAppearance
_hidden
_ignoresKeyboardForPositioning
__shouldAllowNilParameters
_hasPreservedInputViews
__shouldFlipFrameForShimDismissal
_actions
_preferredAction
__presentationSourceRepresentationView
__visualStyle
__compatibilityPopoverController
__systemProvidedPresentationView
__systemProvidedPresentationDelegate
_systemProvidedGestureRecognizer
_temporaryAnimationCoordinator
_previewInteractionController
```
