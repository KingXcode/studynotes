&emsp;&emsp;OC是一种动态语言,不像其他语言的函数调用,而是有自己的一套消息发送的组件,Oc的重要工作依赖于Runtime(运行时)运行的,运行时应该执行的代码由运行环境来决定.

&emsp;&emsp;OC是C语言的超集.

&emsp;&emsp;OC中创建的对象都存在于堆中,比如说我创建一个字符串对象`NSString *string = @"Objective - C";`这个行语句中,等号右边是@"Objective - C"对象,会在堆中创建这个字符串对象拥有自己的一块堆中的内存地址,而等号左边的string是被创建在栈中的一个指针,这个指针存储的是右边字符串对象的地址.32位的计算机,一个指针会占用4个字节的地址,64位计算机,一个指针会占用8个字节的地址.而如果我在创建`NSString *newString = string;`这样只不过是又再栈中又创建一个指针,指向string指向的那块字符创对象的地址,他们存储的值是一样的,但是这两个指针的地址不一样;

&emsp;&emsp;在OC中类似于CGRect这样的数据结构,本质上就是C语言中的结构体,他们也是创建在栈中的.

&emsp;&emsp;在OC中也尽量在.h文件中不要过多的引入其它头文件,如果条件允许的话,用@class来预先声明这个对象,这叫做  向前声明  .

&emsp;&emsp;而在OC中应该尽量使用字面量语法,就像是 `NSString *string = @"wo";`这样的写法,而不是`NSString *string = [NSString stringWithFormat:@"wo"];`这两个写法是等价的,但是在用上面的字面量语法能够让读程序的人看程序更清晰,更重要的是,用字面量语法生成的源代码更短

&emsp;&emsp;以下是OC编译后生成的底层代码,第一行是`NSString *string = @"wo";`生成的,由此可见编译效果用字面量语法更佳.

&emsp;&emsp;而后面用对象方法生成的源代码更长.

&emsp;&emsp;后来我在实际中发现只是在NSString这个类型的数据会缩减源代码,array,dict等等并不会缩短,反而源代码会更长,这里说明OC底层是对string这个类型的对象做过一些内存和代码上的优化的.

```
NSString *string = (NSString *)&__NSConstantStringImpl__var_folders_rx_l216t3ws3yv2b58z7dpkr8zr0000gn_T_main_eaaf03_mi_0;
NSString *string = ((NSString *(*)(id, SEL, NSString *, ...))(void *)objc_msgSend)((id)objc_getClass("NSString"), sel_registerName("stringWithFormat:"), (NSString *)&__NSConstantStringImpl__var_folders_rx_l216t3ws3yv2b58z7dpkr8zr0000gn_T_main_81564b_mi_0);
NSArray *arrayA = [NSArray arrayWithObject:@"wo"];
NSArray *arrayB = @[@"wo"];
//以下是生成的C++源代码,可见用字面量语法的不一样
NSArray *arrayA = ((NSArray *(*)(id, SEL, ObjectType))(void *)objc_msgSend)((id)objc_getClass("NSArray"), sel_registerName("arrayWithObject:"), (id)(NSString *)&__NSConstantStringImpl__var_folders_rx_l216t3ws3yv2b58z7dpkr8zr0000gn_T_main_7528cd_mi_0);

NSArray *arrayB = ((NSArray *(*)(id, SEL, const ObjectType *, NSUInteger))(void *)objc_msgSend)(objc_getClass("NSArray"), sel_registerName("arrayWithObjects:count:"), (const id *)__NSContainer_literal(1U, (NSString *)&__NSConstantStringImpl__var_folders_rx_l216t3ws3yv2b58z7dpkr8zr0000gn_T_main_7528cd_mi_1).arr, 1U);
```



这里关于OC数组再提及一下,

```
假设我现在有三个对象 obj1,obj2,obj3,obj4

NSArray *arrayA = [NSArray arrayWithObjects:obj1,obj2,obj3,obj4,nil];

NSArray *arrayB = @[obj1,obj2,obj3,obj4];

当我的obj3 = nil;这时arrayB会抛出异常,但是arrayA并不会,但是结果有出入,
不是我们想要的结果他会在obj3时截止,不再包含obj4,
这时因为用这个arrayWithObjects:方法是用nil来识别数组对象是否截止,
这里如果我们是用字面量方法来创建的对象,我们就能直接更快的发现错误.所以说用字面量语法更加安全.
```

关于字典如果我们直接使用OC字典的方法创建字典,我们会发现我们所理解的{<键>:<值>}在这里是反着的,我们在写代码的时候是先写值,在写键,我感觉是有点反人类的,但是如果我们是用字面量语法的话,会发现这整个世界都正常了.

并且和数组一样,一旦我们的值是nil,字面量语法创建的话,就会立即抛出异常,但是如果是使用对象方法创建的话就会在这里停止,不再包含之后的数据.

OC中,"对象"就是"基本的构造单元",我们开发时可以通过对象来存储和传递数据.在对象之间传递数据并且执行任务的过程中就叫做"消息传递".在程序运行期间,为其提供相关支持的代码叫做RunTime.
