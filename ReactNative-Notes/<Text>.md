>在 React Native 用于显示文本的组件就是 Text，和iOS中的 UIlabel类似

作为笔记,我只记录部分Text组件的部分常用属性和方法.

```



class customView extends Component {

  clickedHandle() {
      alert("点击");
  }

  render() {
    return (
        <View style={styles.container}>
            <Text onPress={this.clickedHandle}      
                  style={styles.textStyle}>ReactNative</Text>
        </View>
    );
  }
};

var styles = StyleSheet.create({
        container: {
            flex: 1,
            justifyContent: 'center',
            alignItems: 'center',
            backgroundColor: 'green',
        },

        textStyle: {
            // 字体颜色
            color:'blue'
            // 背景色
            backgroundColor:'yellow',
            // 字体大小
            fontSize:30,
            // 下划横线
            textDecorationLine:'underline',
            // 显示文本的行数,如果显示的内容超过行数，默认其余的文本信息不再显示
            numberOfLines:3,
            // 字体类型
            fontFamily:'Georgia',
            // 字体风格('normal', 'italic')
            fontStyle:'italic',
            // 字体粗细（'normal', 'bold', '100 ~ 900'）：指定字体的粗细。
            // 大多数字体都支持'normal'和'bold'值。
            // 并非所有字体都支持所有的数字值。如果某个值不支持，则会自动选择最接近的值
            fontWeight:('bold', '700'),
            // 阴影
            textShadowOffset:{width:3, height:5},
            // 阴影颜色
            textShadowColor:'black',
            // 阴影圆角(值越大阴影越模糊)
            textShadowRadius:3,
            // 字符间距
            letterSpacing:5,
            // 行高
            lineHeight:25,
            // 文本对齐方式
            //（'auto', 'left', 'right', 'center', 'justify'）
            textAlign:'auto',
            // 横线（'none', 'underline', 'line-through删除线'）
            textDecorationLine:'underline',
            // 横线风格（'solid单线', 'double双线', 'dotted短虚线', 'dashed长虚线'）
            textDecorationStyle:'solid',
            // 线的颜色
            textDecorationColor:'black',
            // 决定用户是否可以长按选择文本，以便复制和粘贴
            // =true,长按会显示复制
            selectable:true,
            // 指定字体是否随着给定样式的限制而自动缩放
            adjustsFontSizeToFit:true,
            // 当adjustsFontSizeToFit开启时，指定最小的缩放比（即不能低于这个值）。
            // 可设定的值为0.01 - 1.0
            minimumFontScale:0.1,
        }

    });

```
