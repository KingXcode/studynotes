
&emsp;&emsp;当我们给一个对象发送了一个没有的方法,系统会抛出异常并打印,[unrecognized selector sent to instance ox...]这个异常,意思就是给这个实例对象发送了一个不能识别的消息,这个异常就是由OC中的消息转发机制来实现的,就是在你发送了这个不能识别的消息后,将这个消息转发给了NSObject来实现,前面的这个异常消息就是由NSObject的"doesNotRecognizeSelector:"方法所抛出来的.

&emsp;&emsp;OC中的消息转发分为两大阶段.

&emsp;&emsp;第一阶段是先征询接受者对象所属的类,看这个类是否能动态的添加方法,来处理当前这个"不能识别的方法",这叫做"动态解析".

&emsp;&emsp;第二阶段就涉及到了"完整的消息转发机制".如果Runtime系统已经将第一阶段执行完,那么接受者对象自己就无法再以动态新增方法的手段来响应包含该方法的消息了.这个时候,Runtime系统会请求接受者以其他的手段来处理与消息相关的方法调用.`(这个其他手段又可以细分为两小步:首先,请接收者看看有没有其他对象能处理这条消息.若有,则Runtime系统会把消息转给那个对象,于是消息转发结束,一切如常.若没有"备援的接收者"(replacement receiver),则启动完整的消息转发机制,Runtime系统会把与消息有关的全部细节都封装到NSInvocation对象中,再给接收者最后一次机会,令其设法解决当前还没有处理的这条消息.)`

>关于第一阶段的动态解析：

&emsp;&emsp;对象在收到无法解读的消息后,首先调用其所属类的下列方法:    
&emsp;&emsp;`+(BOOL)resolveInstanceMethod:(SEL)selector;`   
&emsp;&emsp;这个方法的参数就是那个未知的方法,其返回值是Boolean类型,便是这个类能不能新增一个实例方法.在继续往下指向转发机制之前,本类有机会新增一个处理这个方法的方法.------加入这个未知的方法不是实例方法而是类方法,就会调用+(BOOL)resolveClassMethod:(SEL)selector;不过要使用这种方法的前提是:相关的方法的实现代码已经写好,只等着运行的时候动态插在类里面就可以了.

>关于第二阶段的关于备援接收者:

&emsp;&emsp;当前接收者还有第二次机会能处理未知的方法,在这一步中,运行期系统会问它:能不能把这条消息转给其他接收者来处理.Runtime系统会调用一下方法来询问:     
&emsp;&emsp;`-(id)forwardingTargetForSelector:(SEL)selector;`     
&emsp;&emsp;这个方法参数代表位置的方法,若当前接收者能找到备援对象,则返回这个对象.如果找不到,就会返回nil;


>关于完整得分消息转发:

&emsp;&emsp;如果转发算法已经来到这一步的话,那么唯一能做的就是启用完整的消息转发机制了.首先创建一个NSInvocation对象,把没有处理的那条消息有关的全部细节全都封装在这里面.这个对象包含方法/目标(target)/参数.再出发NSInvocation对象时,"消息派发系统"将亲自把消息派给目标对象(会调用下面方法).`-(void)forwaedInvocation:(NSInvocation *)invocation;`   
&emsp;&emsp;这个方法,只需要调用目标,使消息在新目标上得以调用.如果最后还是不能调用的话,NSObject的"doesNotRecognizeSelector:"方法会抛出异常.



关于实例方法  
`[[[Person alloc] init] person];`   
如果我这个实例方法,只声明了方法却没有实现,系统就会从本类调用doesNotRecognizeSelector:方法,如果没有实现这个方法就会一层一层从父类中去找,直到在NSObject中调用,并输出错误信息.

```
2015-12-12 17:46:24.862 mian[4923:1367102] Person++++++++++++++resolveInstanceMethod:
2015-12-12 17:46:26.260 mian[4923:1367102] Person----------forwardingTargetForSelector:
2015-12-12 17:46:27.885 mian[4923:1367102] Person++++++++++++++resolveInstanceMethod:
2015-12-12 17:46:29.194 mian[4923:1367102] Person----------doesNotRecognizeSelector:
```
