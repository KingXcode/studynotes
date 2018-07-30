[Git权威指南](http://www.worldhello.net/gotgit/index.html)
# Git初始化

## 创建版本库以及第一次提交

本章主要学习：`git init`、`git add`和`git commit`

```
git --version //获取git当前版本
```
设置Git的环境变量，这是一次性的设置。即这些设置会在全局文件（用户主目录下的`.gitconfig`）或系统文件（`/etc/gitconfig`）中做永久的记录。

- 告诉Git当前用户的姓名和邮件地址，配置的用户名和邮件地址将在版本库提交时作为提交者的用户名和邮件地址。

    注意下面的两条命令不要照抄照搬，而是用您自己的用户名和邮件地址代替这里的用户名和邮件地址，否则您的劳动成果（提交内容）可要算到作者的头上了。
    
    ```
    $ git config --global user.name "Jiang Xin"
    $ git config --global user.email jiangxin@ossxp.com
    ```
    执行下面的命令，删除Git全局配置文件中关于user.name和user.email的设置：
    
    ```
    $ git config --unset --global user.name
    $ git config --unset --global user.email
    ```
    
- 设置一些Git别名，以便可以使用更为简洁的子命令。
    
    - 如果拥有系统管理员权限（可以执行sudo命令），希望注册的命令别名能够被所有用户使用，可以执行如下命令：

        ```
        $ sudo git config --system alias.br branch
        $ sudo git config --system alias.ci "commit -s"
        $ sudo git config --system alias.co checkout
        $ sudo git config --system alias.st "-p status"
        ```
    - 也可以运行下面的命令，只在本用户的全局配置中添加Git命令别名：
    
        ```
        $ git config --global alias.st status
        $ git config --global alias.ci "commit -s"
        $ git config --global alias.co checkout
        $ git config --global alias.br branch
        ```

1. 终端中输入：`git init` 会在当前目录下初始化git仓库，即：创建了隐藏目录`/{path}/.git`。
`.git`版本库目录所在的目录,称为工作区。

2. 假如在这个目录创建文件`hello.txt`，那么现在在git的工作区出现了这个文件，但是文件还没有添加进入工作区。终端中输入：`git add hello.txt`将这个文件进行标记（后面会提到，即：放入暂存区），继续输入`git commit -m "说明描述"`，将文件添加进本地的版本库中。
    - `git add hello.txt`这个只是标记一个文件如果文件非常多如果这样一个一个的添加未免有些麻烦，可以使用`git add .`来进行批量操作。

## Git暂存区


