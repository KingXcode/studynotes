```
git --version      //获取git当前版本
git init           //将当前目录初始化git仓库
git add <fileName> //将指定文件添加进暂存区
git add .          //将所有有变化的文件添加暂存区
git commit -m "备注"//将暂存区中的变化添加进工作区
git status -s      //将文件的状态精简的输出
```

```
//配置和移除 Git全局配置文件中的name和email
$ git config --global user.name "xxx"
$ git config --global user.email xxx@xxx.com
    
$ git config --unset --global user.name
$ git config --unset --global user.email
```

>可以看出reset命令，大部分都不会影响工作区

```git
/// 仅用HEAD指向的目录树重置暂存区，工作区不会受到影响。相当于将之前用git add命令更新到暂存区的内容撤出暂存区。
git reset   

/// 仅用HEAD指向的目录树重置暂存区，工作区不会受到影响。相当于将之前用git add命令更新到暂存区的内容撤出暂存区。       
git reset HEAD  

/// 仅将文件filename撤出暂存区，暂存区中其他文件不改变。相当于对命令git add filename的反向操作。
git reset – filename  

/// 撤销最新的一次commit。工作区和暂存区不改变，但是版本库的引用向前回退一次。
git reset –soft HEAD^ 

/// 撤销最新的一次add和commit。工作区不改变，但是暂存区会回退到上一次提交之前，引用也会回退一次。
git reset HEAD^  

/// 彻底撤销最近的提交。引用回退到前一次，而且工作区和暂存区都会回退到上一次提交的状态。自上一次以来的提交全部丢失。
git reset –hard HEAD^  
```

> 重置命令(reset)一般用于重置暂存区（除非使用`--hard`参数，否则不重置工作区）

> 检出命令(checkout)主要是覆盖工作区（如果`<commit>`不省略，也会替换暂存区中相应的文件)

```git
/// 将分支切换到master(master是分支名)
git checkout master  

/// 放弃单个文件的修改(filename是文件名)
git checkout filename 

/// 放弃当前目录下的修改。这条命令最危险！会取消所有本地的修改（相对于暂存区）。
/// 相当于将暂存区的所有文件直接覆盖本地文件，不给用户任何确认的机会！
git checkout . 
```

