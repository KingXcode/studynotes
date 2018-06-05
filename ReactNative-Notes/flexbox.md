> FlexBox是前端领域CSS中的一种布局方案，在ReactNative中的Flebox布局方案由于移动端和PC端的操作方式和使用习惯存在的巨大差异，所以官方对Flexbox做了一些阉割，来适应移动端的布局方式。更详细的FlexBox布局教程可以参照[Flex 布局教程：语法篇](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)




 #### ReactNative中Flexbox常用的几个属性

 - 容器属性 `flexDirection`、`justifyContent`、`alignItems`、`flexWrap`

 - 元素属性 `alignSelf`、`flex`、`position`

 >React Native中的Flexbox的工作原理和web上的CSS基本一致，当然也存在少许差异。首先是默认值不同：flexDirection的默认值是column而不是row，而flex也只能指定一个数字值。

1. flexDirection：在组件的style中指定flexDirection可以决定布局的主轴。它有两个值:`row`、`column`。一个表示横向布局另一个表示纵向布局。
![](/resources/flexDirection.png)

    ```
    import React, { Component } from 'react';
    import { AppRegistry, View } from 'react-native';

    class FlexDirectionBasics extends Component {
      render() {
        return (
          // 尝试把`flexDirection`改为`column`看看
          <View style={{flex: 1, flexDirection: 'row'}}>
            <View style={{width: 50, height: 50, backgroundColor: 'powderblue'}} />
            <View style={{width: 50, height: 50, backgroundColor: 'skyblue'}} />
            <View style={{width: 50, height: 50, backgroundColor: 'steelblue'}} />
          </View>
        );
      }
    };

    AppRegistry.registerComponent('AwesomeProject', () => FlexDirectionBasics);
    ```

2. justifyContent：在组件的style中指定justifyContent可以决定其子元素沿着主轴的排列方式。子元素是应该靠近主轴的起始端还是末尾段分布呢？亦或应该均匀分布？对应的这些可选项有：`flex-start`、`center`、`flex-end`、`space-around`以及`space-between`。
![](/resources/justifyContent.png)

    ```
    import React, { Component } from 'react';
    import { AppRegistry, View } from 'react-native';

    class JustifyContentBasics extends Component {
      render() {
        return (
          // 尝试把`justifyContent`改为`center`看看
          // 尝试把`flexDirection`改为`row`看看
          <View style={{
            flex: 1,
            flexDirection: 'column',
            justifyContent: 'space-between',
          }}>
            <View style={{width: 50, height: 50, backgroundColor: 'powderblue'}} />
            <View style={{width: 50, height: 50, backgroundColor: 'skyblue'}} />
            <View style={{width: 50, height: 50, backgroundColor: 'steelblue'}} />
          </View>
        );
      }
    };

    AppRegistry.registerComponent('AwesomeProject', () => JustifyContentBasics);    
    ```

3. alignItems：在组件的style中指定alignItems可以决定其子元素沿着次轴（与主轴垂直的轴，比如若主轴方向为row，则次轴方向为column）的排列方式。对应的这些可选项有：`flex-start`、`center`、`flex-end`以及`stretch`。
![](/resources/alignItems.png)

    ```
    import React, { Component } from 'react';
    import { AppRegistry, View } from 'react-native';

    class AlignItemsBasics extends Component {
      render() {
        return (
          // 尝试把`alignItems`改为`flex-start`看看
          // 尝试把`justifyContent`改为`flex-end`看看
          // 尝试把`flexDirection`改为`row`看看
          <View style={{
            flex: 1,
            flexDirection: 'column',
            justifyContent: 'center',
            alignItems: 'center',
          }}>
            <View style={{width: 50, height: 50, backgroundColor: 'powderblue'}} />
            <View style={{width: 50, height: 50, backgroundColor: 'skyblue'}} />
            <View style={{width: 50, height: 50, backgroundColor: 'steelblue'}} />
          </View>
        );
      }
    };

    AppRegistry.registerComponent('AwesomeProject', () => AlignItemsBasics);
    ```

4. flexWrap：水平或垂直布局时：如果子View放不下，则自动换行, 默认为'nowrap'(不换行)
![](/resources/flexWrap.png)

5. alignSelf：设置子布局在交叉轴方向位置
![](/resources/alignSelf.png)

6. flex：权重，类似Android中weight
![](/resources/flex.png)

7. position：定位模式，分为absolute和relative两种
        absolute：绝对定位，相对于原点（左上角）
        relative：相对定位，相对于当前位置

>这里解释下主轴和交叉轴，当排列方向设置为row时，主轴是横向轴，交叉轴是竖向轴；当排列方向设置为column时，主轴是竖向轴，交叉轴是横向轴；
