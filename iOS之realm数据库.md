# iOS之Realm数据库

## 前言

>Realm是由Y Combinator孵化的创业团队开源出来的一款可以用于iOS(同样适用于Swift&Objective-C)和Android的跨平台移动数据库。目前最新版是Realm 3.7.4，支持的平台包括Java，Objective-C，Swift，React Native，Xamarin。    

去年在个人的两个小项目中尝试着用了realm，当时感觉简直就时我的最爱，尤其是像我这种本身对sql不是特别熟悉的人来说，以前在公司的项目中使用数据库的时候，觉得太变态了，当时公司项目设计了一个专门用来管理数据库的类，一开始不是用的fmdb，而是直接使用原生的sql3的动态库，项目做完后，这个数据库管理类里面写满了sql的代码，有的时候对数据库做些小的更改整个人都感觉不好了

## 环境要求
- Xcode 8.3.3 or higher
- Target of iOS 8 or higher, macOS 10.9 or higher, or any version of tvOS or watchOS

## 安装

**Dynamic Framework**

1. 下载最新的[最新的Realm发行版本](https://static.realm.io/downloads/objc/realm-objc-3.7.4.zip?_ga=2.36617128.943646303.1530519023-701635233.1530519023)，并解压

2. 前往Xcode 工程的”General”设置项中，从`ios/dynamic/`, `osx/`, `tvos/` 或者 `watchos/`中将`Realm.framework`拖曳到”Embedded Binaries”选项中。确认**Copy items if needed**被选中后，点击**Finish**按钮

3. 在单元测试 Target 的”Build Settings”中，在”Framework Search Paths”中添加Realm.framework的上级目录；

4. 如果希望使用 Swift 加载 Realm，请拖动Swift/RLMSupport.swift文件到 Xcode 工程的文件导航栏中并选中**Copy items if needed**；

5. 如果在 iOS、watchOS 或者 tvOS 项目中使用 Realm，请在您应用目标的”Build Phases”中，创建一个新的”Run Script Phase”，并将`bash "${BUILT_PRODUCTS_DIR}/${FRAMEWORKS_FOLDER_PATH}/Realm.framework/strip-frameworks.sh"`这条脚本复制到文本框中。 因为要绕过[APP商店提交的bug](https://link.jianshu.com/?t=http://www.openradar.me/radar?id=6409498411401216)，这一步在打包通用设备的二进制发布版本时是必须的。

**CocoaPods**
在项目的Podfile中，添加pod 'Realm'，在终端运行pod install


## Realm 中的相关术语

- **RLMRealm**：Realm是框架的核心所在，是我们构建数据库的访问点，就如同Core Data的管理对象上下文（managed object context）一样。出于简单起见，realm提供了一个默认的defaultRealm( )的便利构造器方法。

- **RLMObject**：这是我们自定义的Realm数据模型。创建数据模型的行为对应的就是数据库的结构。要创建一个数据模型，我们只需要继承RLMObject，然后设计我们想要存储的属性即可。

- **关系(Relationships)**：通过简单地在数据模型中声明一个RLMObject类型的属性，我们就可以创建一个“一对多”的对象关系。同样地，我们还可以创建“多对一”和“多对多”的关系。

- **写操作事务(Write Transactions)**：数据库中的所有操作，比如创建、编辑，或者删除对象，都必须在事务中完成。“事务”是指位于write闭包内的代码段。

- **查询(Queries)**：要在数据库中检索信息，我们需要用到“检索”操作。检索最简单的形式是对Realm( )数据库发送查询消息。如果需要检索更复杂的数据，那么还可以使用断言（predicates）、复合查询以及结果排序等等操作。

- **RLMResults**：这个类是执行任何查询请求后所返回的类，其中包含了一系列的RLMObject对象。RLMResults和NSArray类似，我们可以用下标语法来对其进行访问，并且还可以决定它们之间的关系。不仅如此，它还拥有许多更强大的功能，包括排序、查找等等操作。


## 开始
##### 1.创建数据库

```objc
- (void)creatDataBaseWithName:(NSString *)databaseName
{
    NSArray *docPath = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES);
    NSString *path = [docPath objectAtIndex:0];
    NSString *filePath = [path stringByAppendingPathComponent:databaseName];
    
    RLMRealmConfiguration *config = [RLMRealmConfiguration defaultConfiguration];
    config.fileURL = [NSURL URLWithString:filePath];
    config.readOnly = NO;
    int currentVersion = 1.0;
    config.schemaVersion = currentVersion;
    config.migrationBlock = ^(RLMMigration *migration , uint64_t oldSchemaVersion) {
        if (oldSchemaVersion < currentVersion) {
            // 这里是设置数据迁移的block
        }
    };
    [RLMRealmConfiguration setDefaultConfiguration:config];
}
```

##### 2.建表

Realm数据模型是基于标准 Objective‑C 类来进行定义的，使用属性来完成模型的具体定义。
我们只需要继承 RLMObject或者一个已经存在的模型类，您就可以创建一个新的 Realm 数据模型对象。对应在数据库里面就是一张表。        

```objc
#import <Realm/Realm.h>
#import <Foundation/Foundation.h>

@interface Test : RLMObject

@end
RLM_ARRAY_TYPE(Test)

@interface HTMainAccountsSubModel : RLMObject
@property  NSString * subTitle;//辅助密码名称  *******  必须不为空
@property  NSString * password;//辅助密码
@property (readonly) RLMLinkingObjects *owners;
@property  Test * test;
@end
RLM_ARRAY_TYPE(HTMainAccountsSubModel)

@interface HTMainAccountsModel : RLMObject
@property  NSString * k_push_id;    //分类跳转使用的id    默认为@"acount"
@property  NSString * k_id;         //分类id   *******   必须不为空 默认为 @"0"
@property  NSString * a_id;         //账号id   *******   必须不为空
@property  NSString * accountTitle; //标题  *******      必须不为空

@property  NSString * creatTime;    //创建时间
@property  NSString * changeTime;   //修改时间

@property  NSString * account;      //账号            可以为空
@property  NSString * password;     //密码            可以为空
@property  NSString * remarks;      //备注            可以为空
@property  BOOL       isCollect;    //是否被收藏
@property  NSInteger  iconType;     //icon类型,实际上只是为了取图标使用  默认为0;

@property  RLMArray<HTMainAccountsSubModel> *infoPassWord;//辅助密码信息 例如:交易密码,其它非登录密码信息
@end
RLM_ARRAY_TYPE(HTMainAccountsModel)
```

    
```
@implementation HTMainAccountsSubModel
//一般来说,属性为nil的话realm会抛出异常,但是如果实现了这个方法的话,就只有subTitle为nil会抛出异常,也就是说现在其他属性可以为空了
+ (NSArray *)requiredProperties {
    return @[@"subTitle"];
}
//设置属性默认值
+ (NSDictionary *)defaultPropertyValues{
    return @{@"subTitle":@"密码"
             };
}
//链接反向关系
+ (NSDictionary *)linkingObjectsProperties {
    return @{
             @"owners": [RLMPropertyDescriptor descriptorWithClass:HTMainAccountsModel.class propertyName:@"infoPassWord"],
             };
}
@end

@implementation HTMainAccountsModel
//一般来说,属性为nil的话realm会抛出异常,但是如果实现了这个方法的话,就只有accountTitle等等为nil会抛出异常,也就是说现在其他属性可以为空了
+ (NSArray *)requiredProperties {
    return @[@"accountTitle",
             @"k_push_id",
             @"a_id",
             @"k_id"];
}
// 主键
+ (NSString *)primaryKey {
    return @"a_id";
}
//设置属性默认值
+ (NSDictionary *)defaultPropertyValues{
    return @{@"isCollect":@(NO),
             @"iconType":@(0),
             @"k_push_id":@"acount",
             @"k_id":@"0"
             };
}
@end
```
还可以给RLMObject设置主键primaryKey，默认值defaultPropertyValues，忽略的属性ignoredProperties，必要属性requiredProperties，索引indexedProperties。比较有用的是主键和索引      

注意，RLMObject 官方建议不要加上 Objective-C的property attributes(如nonatomic, atomic, strong, copy, weak 等等）假如设置了，这些attributes会一直生效直到RLMObject被写入realm数据库。      

RLM_ARRAY_TYPE宏创建了一个协议，从而允许`RLMArray<HTMainAccountsSubModel>`语法的使用。如果该宏没有放置在模型接口的底部的话，您或许需要提前声明该模型类。    

- 关于RLMObject的的关系

    1. 对一(To-One)关系
    ```
    对于多对一(many-to-one)或者一对一(one-to-one)关系来说，只需要声明一个RLMObject子类类型的属性即可，如上面代码例子，@property  Test * test; 
    ```
    2. 对多(To-Many)关系
    ```
    通过 RLMArray类型的属性您可以定义一个对多关系。如上面代码例子，@property  RLMArray<HTMainAccountsSubModel> *infoPassWord;
    ```
    3. 反向关系(Inverse Relationship)
   
    ```
    链接是单向性的。
    因此，如果对多关系属性 HTMainAccountsModel.infoPassWord链接了一个 HTMainAccountsSubModel实例，
    而这个实例的对一关系属性 HTMainAccountsSubModel.owner又链接到了对应的这个 HTMainAccountsModel实例，
    那么实际上这些链接仍然是互相独立的。
    ```
    
    ```objc
    //反向关系
    + (NSDictionary *)linkingObjectsProperties {
    return @{
             @"owners": [RLMPropertyDescriptor descriptorWithClass:HTMainAccountsModel.class propertyName:@"infoPassWord"],
             };
}
    ```
    

##### 3.创建对象

```objc
// (1) 创建一个HTMainAccountsModel对象，然后设置其属性
HTMainAccountsModel *model = [[HTMainAccountsModel alloc] init];
model.k_push_id = @"";
model.k_id = @"";
model.a_id = @"";
model.creatTime = @"";


// (2) 通过字典创建HTMainAccountsModel对象
HTMainAccountsModel *model = [[HTMainAccountsModel alloc]initWithValue:@{@"k_push_id":self.item.k_push_id,@"k_id":self.item.k_id,"a_id":@(creatTime).stringValue,@"creatTime":@(creatTime).stringValue}];

// (3) 通过数组创建HTMainAccountsModel对象
HTMainAccountsModel *model = [[HTMainAccountsModel alloc] initWithValue:@[]];//这行完全就只是示例

```

##### 4.增
```objc
//addObject
RLMRealm *realm = [RLMRealm defaultRealm];
[realm beginWriteTransaction];
[realm addObject:model];
[realm commitWriteTransaction];

//addOrUpdateObject 当realm中已经存在这个对象则调用这个方法
//addOrUpdateObject会去先查找有没有传入的model相同的主键，如果有，就更新该条数据。这里需要注意，addOrUpdateObject这个方法不是增量更新，所有的值都必须有，如果有哪几个值是null，那么就会覆盖原来已经有的值，这样就会出现数据丢失的问题。
RLMRealm *realm = [RLMRealm defaultRealm];
[realm beginWriteTransaction];
[realm addOrUpdateObject:model];
[realm commitWriteTransaction];
```


##### 5.删
```
[realm beginWriteTransaction];
// 删除单条记录
[realm deleteObject:model];
// 删除多条记录
[realm deleteObjects:modelResult];
// 删除所有记录
[realm deleteAllObjects];

[realm commitWriteTransaction];
```

##### 6.改
```
[realm addOrUpdateObject:model];

[HTMainAccountsModel createOrUpdateInRealm:realm withValue:@{@"k_id": @1}];
```

##### 7.查
在Realm中所有的查询（包括查询和属性访问）在 Realm 中都是延迟加载的，只有当属性被访问时，才能够读取相应的数据。   

查询结果并不是数据的拷贝：修改查询结果（在写入事务中）会直接修改硬盘上的数据。同样地，您可以直接通过包含在RLMResults中的RLMObject对象完成遍历关系图的操作。除非查询结果被使用，否则检索的执行将会被推迟。这意味着链接几个不同的临时 {RLMResults} 来进行排序和匹配数据，不会执行额外的工作，例如处理中间状态。         

一旦检索执行之后，或者通知模块被添加之后， RLMResults将随时保持更新，接收 Realm 中，在后台线程上执行的检索操作中可能所做的更改。

```
//从默认数据库查询所有的车
RLMResults<HTMainAccountsModel *> *model = [HTMainAccountsModel allObjects];

// 使用断言字符串查询
RLMResults<HTMainAccountsModel *> *model = [HTMainAccountsModel objectsWhere:@"kid = '' AND accountTitle BEGINSWITH 'm'"];

// 使用 NSPredicate 查询
NSPredicate *pred = [NSPredicate predicateWithFormat:@"kid = %@ AND accountTitle BEGINSWITH %@",@"1", @"mm"];
RLMResults *results = [HTMainAccountsModel objectsWithPredicate:pred];

// 排序
RLMResults<HTMainAccountsModel *> *model = [[HTMainAccountsModel objectsWhere:@"kid = '11' AND accountTitle BEGINSWITH 'm'"] sortedResultsUsingProperty:@"a_id" ascending:YES];
```


#### - 跨线程访问数据库，Realm对象一定需要新建一个,自己封装一个Realm全局实例单例是没啥作用的
很多开发者应该都会对Core Data和Sqlite3或者FMDB，自己封装一个类似Helper的单例。于是我也在这里封装了一个单例，在新建完Realm数据库的时候strong持有一个Realm的对象。然后之后的访问中只需要读取这个单例持有的Realm对象就可以拿到数据库了。  

想法是好的，但是同一个Realm对象是不支持跨线程操作realm数据库的。   

Realm 通过确保每个线程始终拥有 Realm 的一个快照，以便让并发运行变得十分轻松。你可以同时有任意数目的线程访问同一个 Realm 文件，并且由于每个线程都有对应的快照，因此线程之间绝不会产生影响。需要注意的一件事情就是不能让多个线程都持有同一个 Realm 对象的 实例 。如果多个线程需要访问同一个对象，那么它们分别会获取自己所需要的实例（否则在一个线程上发生的更改就会造成其他线程得到不完整或者不一致的数据）。    

其实RLMRealm *realm = [RLMRealm defaultRealm]; 这句话就是获取了当前realm对象的一个实例，其实实现就是拿到单例。所以我们每次在子线程里面不要再去读取我们自己封装持有的realm实例了，直接调用系统的这个方法即可，能保证访问不出错。  

#### transactionWithBlock 已经处于一个写的事务中，事务之间不能嵌套
```
[realm transactionWithBlock:^{
                [self.realm beginWriteTransaction];
                [self convertToRLMUserWith:bhUser To:[self convertToRLMUserWith:bhUser To:nil]];
                [self.realm commitWriteTransaction];
            }];
```

#### 建议每个model都需要设置主键，这样可以方便add和update
```
如果能设置主键，请尽量设置主键，
因为这样方便我们更新数据，
我们可以很方便的调用addOrUpdateObject: 或者 createOrUpdateInRealm：withValue：方法进行更新。
这样就不需要先根据主键，查询出数据，
然后再去更新。
有了主键以后，这两步操作可以一步完成。
```


