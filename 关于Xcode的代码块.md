>关于Xcode的代码块，我感觉应该是这个编译器里最好用的部分的，用的好的话对你开发的速度是成十倍百倍的效率提升。

&emsp;&emsp;将近3年的Xcode的使用经验，我自己多少也总结了不少自己常用的代码块，请将目光向下移动。

- 快速创建数组属性

    快捷键：`ht_array`

    输出：`@property (nonatomic,copy) NSArray * <#name#>;`

- 快速创建基本数据类型属性

    快捷键：`ht_assign`

    输出：`@property (nonatomic,assign) <#class#> <#name#>;`

- 快速创建字符属性

    快捷键：`ht_string`

    输出：`@property (nonatomic,copy) NSString * <#name#>;`

- 快速创建强引用属性

    快捷键：`ht_strong`

    输出：`@property (nonatomic,strong) <#class#> * <#name#>;`

- 快速创建弱引用属性

    快捷键：`ht_weak`

    输出：`@property (nonatomic,weak) <#class#> * <#name#>;`

- 快速创建弱引用指针

    快捷键：`ht_weak_self`

    输出：`__weak typeof(self) __self = self;`


- 快速创建代码块属性

    快捷键：`ht_block`

    输出：`@property (nonatomic, copy) void(^<#name#>)();`

- 截取系统侧滑手势[备注：有些特殊情况下需要禁用掉系统的侧滑功能，但是有的时候直接禁用nav的侧滑手势在某些可能下会造成系统的假死（我遇到过很多次），所以，这是我经常使用的方法]，这个方法就是通过添加手势来截断系统的手势。

    快捷键：`ht_cehua`

    输出：
`UIScreenEdgePanGestureRecognizer *ges = [[UIScreenEdgePanGestureRecognizer alloc] initWithTarget:self action:nil];`
`ges.edges = UIRectEdgeLeft;// 指定左边缘滑动`
`[self.view addGestureRecognizer:ges];`


- 快速创建按钮

    快捷键：`ht_creatButton`

    输出：

  `UIButton *<#button#> = [UIButton buttonWithType:UIButtonTypeCustom];`
`[<#button#> setTitleColor:[MyColor pg_mainTitleColor] forState:UIControlStateNormal];`
`<#button#>.titleLabel.font = [UIFont systemFontOfSize:<#size#>];`
`[<#button#> setTitle:@"" forState:UIControlStateNormal];`
`<#button#>.contentHorizontalAlignment = UIControlContentHorizontalAlignmentLeft;`


- 键盘监听,记得移除监听

    快捷键：`ht_keyboarderObeserver`

    输出：
    ```
    [[NSNotificationCenter defaultCenter] addObserver:self
                                             selector:@selector(keyboardWillShow:)
                                                 name:UIKeyboardWillShowNotification
                                               object:nil];
    [[NSNotificationCenter defaultCenter] addObserver:self
                                              selector:@selector(keyboardWillHide:)
                                                  name:UIKeyboardWillHideNotification
                                                object:nil];
    - (void)keyboardWillShow:(NSNotification *)notification
    {
        //获取键盘的高度
        NSDictionary *userInfo = [notification userInfo];
        NSValue *value = [userInfo objectForKey:UIKeyboardFrameEndUserInfoKey];
        CGRect keyboardRect = [value CGRectValue];
        int height = keyboardRect.size.height;

    }

    //当键退出
    - (void)keyboardWillHide:(NSNotification *)notification
    {
        //获取键盘的高度
        NSDictionary *userInfo = [notification userInfo];
        NSValue *value = [userInfo objectForKey:UIKeyboardFrameEndUserInfoKey];
        CGRect keyboardRect = [value CGRectValue];
        int height = keyboardRect.size.height;

    }
    ```

- 快速创建按钮

    快捷键：`ht_creatTableViewHeaderBtn`

    输出：
    ```
    UIButton *button = [UIButton buttonWithType:UIButtonTypeCustom];
    [button setTitleColor:<#Color#> forState:UIControlStateNormal];
    [button setBackgroundColor:<#Color#>];
    button.titleLabel.font = [UIFont systemFontOfSize:<#FontSize#>];
    [button setTitle:@"" forState:UIControlStateNormal];
    button.contentHorizontalAlignment = UIControlContentHorizontalAlignmentLeft;
    [button setContentEdgeInsets:UIEdgeInsetsMake(0, 15, 0, 0)];
    return button;
    ```


- 快速创建tableview

    快捷键：`ht_creatTableView`

    输出：
    ```
    UITableView *tableView = [[UITableView alloc]initWithFrame:CGRectZero style:UITableViewStylePlain];
    tableView.separatorStyle = UITableViewCellSeparatorStyleNone;
    tableView.delegate = self;
    tableView.dataSource = self;
    tableView.bounces = YES;
    tableView.keyboardDismissMode = UIScrollViewKeyboardDismissModeOnDrag;
    tableView.backgroundColor = <#Color#>;
    tableView.estimatedRowHeight = 0;
    tableView.estimatedSectionHeaderHeight = 0;
    tableView.estimatedSectionFooterHeight = 0;
    self.tableView = tableView;
    [self.view addSubview:tableView];
    [tableView mas_makeConstraints:^(MASConstraintMaker *make) {
        make.top.mas_equalTo(self.navigationView.mas_bottom);
        make.left.bottom.right.equalTo(self.view);
    }];
    ```


- 快速创建tableview代理方法

    快捷键：`ht_creatTableView_delegate`

    输出：

```
#pragma -mark- tableView delegate  datasuoce
-(NSInteger)numberOfSectionsInTableView:(UITableView *)tableView
{
    return 1;
}

-(NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
{
    return <#count#>;
}

-(UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    return <#cell#>;
}

-(CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath
{
    return <#count#>;
}

-(void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath
{
    [tableView deselectRowAtIndexPath:indexPath animated:YES];
}
```


- 快速创建collectionView

    快捷键：`ht_creatCollectionView`

    输出：

```
UICollectionViewFlowLayout *flowLayout = [[UICollectionViewFlowLayout alloc]init];
flowLayout.headerReferenceSize = CGSizeMake(<#width#>, <#height#>);
flowLayout.footerReferenceSize = CGSizeMake(<#width#>, <#height#>);

UICollectionView *collectionView = [[UICollectionView alloc]initWithFrame:CGRectZero collectionViewLayout:flowLayout];
collectionView.delegate = self;
collectionView.dataSource = self;
collectionView.backgroundColor = [UIColor whiteColor];

[collectionView registerNib:[UINib nibWithNibName:@"" bundle:nil] forCellWithReuseIdentifier:@"cell"];
[collectionView registerNib:[UINib nibWithNibName:@"" bundle:nil] forSupplementaryViewOfKind:UICollectionElementKindSectionHeader withReuseIdentifier:@"head"];
[collectionView registerNib:[UINib nibWithNibName:@"" bundle:nil] forSupplementaryViewOfKind:UICollectionElementKindSectionFooter withReuseIdentifier:@"footer"];
[self.view addSubview:collectionView];

self.flowLayout = flowLayout;
self.collectionview = collectionView;

[collectionView mas_makeConstraints:^(MASConstraintMaker *make) {
    make.top.mas_equalTo(self.navigationView.mas_bottom);
    make.left.bottom.right.equalTo(self.view);
}];
```

- 快速创建collectiongView代理

    快捷键：`ht_creatCollectionView_delegate`

    输出：

```
    - (NSInteger)numberOfSectionsInCollectionView:(UICollectionView *)collectionView
    {
        return <#count#>;
    }

    - (NSInteger)collectionView:(UICollectionView *)collectionView numberOfItemsInSection:(NSInteger)section
    {
        return <#count#>;
    }

    - (__kindof UICollectionViewCell *)collectionView:(UICollectionView *)collectionView cellForItemAtIndexPath:(NSIndexPath *)indexPath
    {
        return nil;
    }

    - (UICollectionReusableView *)collectionView:(UICollectionView *)collectionView viewForSupplementaryElementOfKind:(NSString *)kind atIndexPath:(NSIndexPath *)indexPath;
    {
        //如果是头视图
        if ([kind isEqualToString:UICollectionElementKindSectionHeader])
        {

            return nil;
        }
        else if ([kind isEqualToString:UICollectionElementKindSectionFooter])
        {
            return nil;
        }

        return nil;
    }


    - (void)collectionView:(UICollectionView *)collectionView didSelectItemAtIndexPath:(NSIndexPath *)indexPath;
    {

    }

    - (CGSize)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout*)collectionViewLayout sizeForItemAtIndexPath:(NSIndexPath *)indexPath
    {
        return CGSizeMake(<#width#>, <#height#>);
    }


    - (UIEdgeInsets)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout*)collectionViewLayout insetForSectionAtIndex:(NSInteger)section
    {
        return UIEdgeInsetsMake(0, 0, 0, 0);
    }


    - (CGFloat)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout*)collectionViewLayout minimumLineSpacingForSectionAtIndex:(NSInteger)section
    {
        return 0;
    }


    - (CGFloat)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout*)collectionViewLayout minimumInteritemSpacingForSectionAtIndex:(NSInteger)section
    {
        return 0;
    }

```
