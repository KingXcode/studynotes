关于 NSPredicate
==================
######简述：Cocoa框架中的NSPredicate用于查询，原理和用法都类似于SQL中的where，作用相当于数据库的过滤取。

###常用的表达式
####(1)比较运算符>,<,==,>=,<=,!=,可用于数值及字符串
>=、==：判断两个表达式是否相等，在谓词中=和==是相同的意思都是判断，而不是赋值
例：
  ```objc
  NSNumber *number = @727;
  NSPredicate *predicate = [NSPredicate predicateWithFormat:@"SELF = 728"];
  if ([predicate evaluateWithObject:number]) {
      NSLog(@"testString:%@", number);
  }
  ```

####(2)范围运算符：IN、BETWEEN
例：
```objc
@"number BETWEEN {1,5}"
@"address IN {'shanghai','beijing'}"
```


####(3)字符串本身:SELF
例：
```objc
@"SELF == ‘APPLE’"
```

####(4)字符串相关：BEGINSWITH、ENDSWITH、CONTAINS、LIKE

>`BEGINSWITH` ：检查某个字符串是否以指定的字符串开头

>`ENDSWITH` ：检查某个字符串是否以指定的字符串结尾

>`CONTAINS` ：检查某个字符串是否包含指定的字符串

>`LIKE` ：检查某个字符串是否匹配指定的字符串模板。其之后可以跟`?`代表一个字符和`*`代表一个或者多个或者是空。比如`"name LIKE '*ac*'"`，这表示name的值中包含`ac`则返回YES；`"name LIKE '?ac*'"`，表示name的第2、3个字符为`ac`时返回YES。


例：
```objc
@"name CONTAIN[cd]   'ang'"    //包含某个字符串
@"name BEGINSWITH[c] 'sh'"     //以某个字符串开头
@"name ENDSWITH[d]   'ang'"    //以某个字符串结束
NSString *string = @"abcd BEGINSWITH 'a'";
NSPredicate *predicate = [NSPredicate predicateWithFormat:string];
```
注:
```objc
注： 字符串比较都是区分大小写和重音符号的。
如：café和cafe是不一样的，Cafe和cafe也是不一样的。
如果希望字符串比较运算不区分大小写和重音符号，
请在这些运算符后使用[c]，[d]选项。
其中
[c]不区分大小写
[d]不区分发音符号即没有重音符号
[cd]既不区分大小写，也不区分发音符号。
其写在字符串比较运算符之后，比如："name LIKE[cd] 'cafe'"，
那么不论`name`是cafe、Cafe还是café上面的表达式都会返回YES。
```

####(5)正则表达式：MATCHES

>`MATCHES`：检查某个字符串是否匹配指定的正则表达式。虽然正则表达式的执行效率并不高，但其功能是最强大的，也是我们最常用的。`[c]`,`[d]`,`[cd]`同样适用
例：

```objc
  NSString *regex = @"^A.+e$";   //以A开头，e结尾
  NSString *regex = @"[A-Za-z]+";//判断字符串首字母是否为字母:
  NSPredicate *predicate = [NSPredicate predicateWithFormat:@"SELF MATCHES %@", regex];

```  
正则表达式请参考：[iOS正则表达式的简单使用](http://www.jianshu.com/p/4b2bad4ad3e8)

####(6)集合运算符
>`ANY、SOME` ：集合中任意一个元素满足条件，就返回YES。

>`ALL` ：集合中所有元素都满足条件，才返回YES。

>`NONE` ：集合中没有任何元素满足条件就返回YES。

>`IN`：等价于SQL语句中的IN运算符，只有当左边表达式或值出现在右边的集合中才会返回YES。我们通过一个例子来看一下

```objc
//相同的元素去除
NSArray *filterArray = @[@"ab", @"abc",@"uy"];
NSArray *array       = @[@"a", @"ab", @"abc", @"abcd"];
NSPredicate *predicate = [NSPredicate predicateWithFormat:@"NOT (SELF IN %@)", filterArray];
NSLog(@"%@", [array filteredArrayUsingPredicate:predicate]);
```

####(7)直接量
>`FALSE、NO`：代表逻辑假

>`TRUE、YES`：代表逻辑真

>`NULL、NIL`：代表空值

>`SELF` ：代表正在被判断的对象自身

>` "string"或'string'`：代表字符串

>`数组`：和c中的写法相同，如：`{'one', 'two', 'three'}`。

>`数值`：包括整数、小数和科学计数法表示的形式

>`十六进制数`：0x开头的数字

>`八进制`：0o开头的数字

>`二进制`：0b开头的数字

####(8)保留字 （不区分大小写）
>AND、OR、IN、NOT、ALL、ANY、SOME、NONE、LIKE、CASEINSENSITIVE、CI、MATCHES、CONTAINS、BEGINSWITH、ENDSWITH、BETWEEN、NULL、NIL、SELF、TRUE、YES、FALSE、NO、FIRST、LAST、SIZE、ANYKEY、SUBQUERY、CAST、TRUEPREDICATE、FALSEPREDICATE


###提示
#####实际上在实际开发中OC对几个集合类提供了NSPredicate的过滤方法
- NSArray提供了如下方法使用谓词来过滤集合
`- (NSArray *)filteredArrayUsingPredicate:(NSPredicate *)predicate:`使用指定的谓词过滤NSArray集合，返回符合条件的元素组成的新集合

- NSMutableArray提供了如下方法使用谓词来过滤集合
`- (void)filterUsingPredicate:(NSPredicate *)predicate：`使用指定的谓词过滤NSMutableArray，剔除集合中不符合条件的元素

- NSSet提供了如下方法使用谓词来过滤集合
`- (NSSet *)filteredSetUsingPredicate:(NSPredicate *)predicate NS_AVAILABLE(10_5, 3_0):`作用同NSArray中的方法

- NSMutableSet提供了如下方法使用谓词来过滤集合
`- (void)filterUsingPredicate:(NSPredicate *)predicate NS_AVAILABLE(10_5, 3_0):`作用同NSMutableArray中的方法。


```
1.首先假定有一个对象Person 其中Person有个属性叫做name和age
现在创建Person对象
Person *person = [Person alloc]init];

person.name = @"nie";

NSPredicate *pred = [NSPredicate predicateWithFormat:@"name LIKE 'n*'"];

NSLog(@"%d", [pred evaluateWithObject:person]);

>这里的输出是1.

NSPredicate的实例对象调用的evaluateWithObject方法，
是基于OC对象的，所以说 @"name LIKE 'n*'" 这里的name是对象person中的一个属性，
这里判断的是person对象的name属性是不是以n开头的。
在实际开发中很多情况下做数据的处理是基于我们自定义的对象，
多使用NSPredicate可以有助于实际开发中简化代码的逻辑，
减少使用for循环，if语句这些啰嗦的代码
```
```
如果我们想在谓词表达式中使用变量，那么我们需要了解下列两种占位符：

`%K`：用于动态传入属性名

`%@`：用于动态设置属性值

其实相当于变量名与变量值，除此之外，还可以在谓词表达式中使用动态改变的属性值，就像环境变量一样
NSPredicate *pred = [NSPredicate predicateWithFormat:@"SELF CONTAINS $VALUE"];
上述表达式中，`$VALUE`是一个可以动态变化的值，它其实最后是在字典中的一个key，所以可以根据你的需要写不同的值，但是必须有$开头，随着程序改变`$VALUE`这个谓词表达式的比较条件就可以动态改变。

下面我们通过一个例子来看看这三个重要的占位符应该如何使用

NSArray *array = @[person0,person1,person2];

// 定义一个property来存放属性名，定义一个value来存放值
NSString *property = @"name";
NSString *value = @"nie";

// 该谓词的作用是如果元素中property属性含有值value时就取出放入新的数组内，这里是name包含nie
NSPredicate *pred = [NSPredicate predicateWithFormat:@"%K CONTAINS %@", property, value];
NSArray *newArray = [array filteredArrayUsingPredicate:pred];
NSLog(@"newArray:%@", newArray);

// 创建谓词，属性名改为age，要求这个age包含$VALUE字符串
NSPredicate *predTemp = [NSPredicate predicateWithFormat:@"%K > $VALUE", @"age"];
// 指定$VALUE的值为 25
NSPredicate *pred1 = [predTemp predicateWithSubstitutionVariables:@{@"VALUE" : @25}];
NSArray *newArray1 = [array filteredArrayUsingPredicate:pred1];
NSLog(@"newArray1:%@", newArray1);

// 修改 $VALUE的值为32
NSPredicate *pred2 = [predTemp predicateWithSubstitutionVariables:@{@"VALUE" : @32}];
NSArray *newArray2 = [array filteredArrayUsingPredicate:pred2];
NSLog(@"newArray2:%@", newArray2);
```



###代码应用
####(1)对NSArray进行过滤
```objc
NSArray *array = [[NSArray alloc]initWithObjects:@"beijing",@"shanghai",@"guangzou",@"wuhan", nil];   
NSString *string = @"ang";   
NSPredicate *pred = [NSPredicate predicateWithFormat:@"SELF CONTAINS %@",string];   
NSLog(@"%@",[array filteredArrayUsingPredicate:pred]);
```
####(2)判断字符串首字母是否为字母:
```objc
NSString *regex = @"[A-Za-z]+";
NSPredicate *predicate = [NSPredicate predicateWithFormat:@"SELF MATCHES %@", regex];
if ([predicate evaluateWithObject:@"rrrkmr"]) {
    NSLog(@"1");
}else{
    NSLog(@"2");
}
```
####(3)检查某个字符串是否以指定的字符串开头
```objc
NSString *string = @"abcdefg";
NSPredicate *predicate = [NSPredicate predicateWithFormat:@" %@ BEGINSWITH 'a'",string];
if ([predicate evaluateWithObject:string]) {
    NSLog(@"string:%@", string);
}
```
###(4)对象数组过滤 这个方法太好了,太强大
```objc
Person *model_0 = [[Person alloc]initWithName:@"45xiaomiyang"];
Person *model_1 = [[Person alloc]initWithName:@"shabi"];
Person *model_2 = [[Person alloc]initWithName:@"beijing"];
Person *model_3 = [[Person alloc]initWithName:@"hahayang"];
Person *model_4 = [[Person alloc]initWithName:@"niesiyang"];

NSArray *array = [[NSArray alloc]initWithObjects:model_0,model_1,model_2,model_3,model_4, nil];
NSPredicate *pred = [NSPredicate predicateWithFormat:@"self.name.integerValue <= 20"];
NSLog(@"%@",[array filteredArrayUsingPredicate:pred]);
```

####(5)过滤数组中重复的元素  模型数组通用
```objc
/**
 根据modelArray中的属性 去除重复的元素
 目前我的测试中 NSString 和 int类型是没有问题的,过滤正常.
 感慨一下,系统的数组提供的过滤器,排序方式真的很强大

 @param object 模型数组
 @param keypath 模型数组元素中的一个属性
 @return 过滤之后的新数组
 */
 +(NSArray *)ht_removeRepeatRowsForArray:(NSArray *)object
                             WithKeypath:(NSString *)keypath
 {
     NSMutableSet *seenObjects = [NSMutableSet set];
     NSPredicate * predicate = [NSPredicate predicateWithBlock: ^BOOL(id obj, NSDictionary *bind) {
         id property = [obj valueForKeyPath:keypath];//元素属性
         BOOL seen = [seenObjects containsObject:property];
         if (!seen) {
             [seenObjects addObject:property];
         }
         return !seen;
     }];
     NSMutableArray *categoryArray = [NSMutableArray arrayWithArray:[object filteredArrayUsingPredicate:predicate]];
     return categoryArray;
 }
```
