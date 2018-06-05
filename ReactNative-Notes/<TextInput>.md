TextInput是一个允许用户在应用中通过键盘输入文本的基本组件。本组件的属性提供了多种特性的配置，譬如自动完成、自动大小写、占位文字，以及多种不同的键盘类型（如纯数字键盘）等等。

最简单的用法就是丢一个TextInput到应用里，然后订阅它的onChangeText事件来读取用户的输入。注意，从TextInput里取值这就是目前唯一的做法！也就是使用在onChangeText中用setState把用户的输入写入到state中，然后在需要取值的地方从this.state中取出值。它还有一些其它的事件，譬如onSubmitEditing和onFocus。

### 常用属性

`placeholder`:占位符   
`value`:输入框的值   
`password`:是否密文输入   
`editable`:输入框是否可编辑  
`returnKeyType`:键盘return类型  
`onChange`:当文本变化时调用  
`onEndEditing`:当结束编辑时调用  
`onSubmitEditing`:当结束编辑,点击提交按钮时调用   
