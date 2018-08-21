```
git --version      //获取git当前版本
git init           //将当前目录初始化git仓库
git add <fileName> //将指定文件添加进暂存区
git add .          //将所有有变化的文件添加暂存区
git commit -m "备注"//将暂存区中的变化添加进工作区
git status -s      //将文件的状态精简的输出
git push origin master  //将本地仓库的master分支推送合并到远程仓库中
git pull origin master  //将远程仓库的master分支合并到本地仓库
git reset //将之前用git add命令更新到暂存区的内容撤出暂存区
git reset - <fileName>  //指定文件将之从暂存区撤销 
git reset HEAD^         //撤销最新的一次add和commit
git checkout .  //放弃当前目录下的修改
git checkout <filename>  //放弃单个文件的修改
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

> 命令`git stash`可以用于保存和恢复工作进度

```git
/// 保存当前工作进度。会分别对暂存区和工作区的状态进行保存。
git stash

/// 显示进度列表。此命令显然暗示了`git stash`可以多次保存工作进度，并且在恢复的时候进行选择。
git stash list

/// 如果不使用任何参数，会恢复最新保存的工作进度，并将恢复的工作进度从存储的工作进度列表中清除
/// 如果提供<stash>参数（来自于git stash list显示的列表），则从该<stash>中恢复。恢复完毕也将从进度列表中删除<stash>
/// 选项--index除了恢复工作区的文件外，还尝试恢复暂存区
git stash pop [–index] [<stash>]

/// 除了不删除恢复的进度之外，其余和git stash pop命令一样。
git stash apply [–index] [<stash>]

/// 删除指定的一个进度，默认删除最新的进度
/// 使用方法如git stash drop stash@{0}
git stash drop

/// 删除所有存储的进度
git stash clear

/// 显示stash的内容具体是什么，默认显示最新的进度
/// 使用方法如 git stash show stash@{0}
git stash show
```


