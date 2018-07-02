# iOS layoutSubviews的作用与调用时机

> -(void)layoutSubviews; 这个方法是UIView中的一个常用方法，这个方法本身没有做任何事情，需要子类进行重写。

##作用

当我们自定义view的时候重写UIView的layoutSubviews如果程序需要对该控件所包含的子控件布局进行更精确的控制可通过重写该方法来实现。

##触发时机
>在苹果的官方文档中已经指出，不要直接调用这个方法对子view进行重新布局。因为这个方法会在必要的时候由系统自发的进行调用，但是通过调用以下的方法会由系统自行触发这个方法进行子view的重新布局。


- 调用`addSubView`方法时会触发

- 设置`view`的`frame`时会触发，但是需要`frame`发生变化

- 滚动`scrollView`会触发（创建的`View`的父视图时`scrollView`，并且进行滚动时） 

- 改变一个`UIView`大小的时候也会触发`父UIView`上的`layoutSubviews`事件

- `init`初始化不会触发`layoutSubviews`，但是`initWithFrame`初始化时，`rect`的值不是`zero`时也会触发

- 直接调用`[self setNeedsLayout]`（这个方法不会立即刷新，如果需要立即刷新还需要调用`[self layoutIfNeeded]`）

- 旋转`Screen`会触发`父UIView`上的`layoutSubviews`事件。`（这个我试过，没有触发，不知道为什么，但是网上很多又说可以，不能确定）`





