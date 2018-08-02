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
当我们在工作去修改了文件之后，我们是不能直接将工作区的文件直接提交到本地的版本库中的，在git中有一个被称之为暂存区的文件，这相当于是工作区与本地版本库中的中间状态（这里称之为状态其实也不合适，但是我也找不到用什么词好）。当我们在工作区的文件进行了修改之后，将修改的文件`add`进暂存区，然后执行`commit`，这样就会将工作区的变动存入版本库中。

1. 工作区中修改了文件`object.h`
    - 执行`git status -s`精简的状态输出

    ```
    $ git status -s
     M object.h
    ```
    
2. 执行命令`git add object.h`，到这一步修改的文件只被提交到在暂存区。
    - 这时执行`git diff`会发现没有输出，因为这是对比的暂存区和版本库中文件的差异。
    - 执行`git diff HEAD`，将当前工作区和HEAD（当前版本库的头指针）进行比较，会发现有差异。这个差异才是正常的，因为尚未真正提交么。
    - 执行`git status -s`精简的状态输出与执行git add之前的精简状态输出相比，有细微的差别

    ```
    $ git status -s
    M  object.h
    ```
    
    ```
    1. 虽然都是 M（Modified）标识，但是位置不一样。在执行git add命令之前，这个M位于第二列（第一列是一个空格），在执行完git add之后，字符M位于第一列（第二列是空白）。 
    2. 位于第一列的字符M的含义是：版本库中的文件和处于中间状态——提交任务（提交暂存区，即stage）中的文件相比有改动。
    3. 位于第二列的字符M的含义是：工作区当前的文件和处于中间状态——提交任务（提交暂存区，即stage）中的文件相比也有改动。
    ```

3. 此时继续修改本地文件`object.h`
    - 执行`git status -s`精简的状态输出，下面表示：不但版本库中最新提交的文件和处于中间状态 —— 提交任务（提交暂存区, stage）中的文件相比有改动，而且工作区当前的文件和处于中间状态 —— 提交任务（提交暂存区, stage）中的文件相比也有改动。
    
    ```
    $ git status -s
    MM welcome.txt
    ```
    
    - 不带任何选项和参数调用`git diff`显示工作区最新改动，即工作区和提交任务（提交暂存区，stage）中相比的差异。
    - 将工作区和HEAD（当前工作分支）相比（`git diff HEAD`），会看到更多的差异。
    - 通过参数`--cached`或者`--staged`参数调用`git diff`命令，看到的是提交暂存区（提交任务，stage）和版本库中文件的差异。

4. 现在执行`git commit -m "备注"`
    - 执行`git status -s`精简的状态输出。此时第一列的M没有了，表示暂存区-版本库之间的变动没有了，因为暂存区中的内容被提交到了版本库。但是第二个M还在，表示工作区的内容和暂存区之间还存在差异，需要将工作区的改动add进入暂存区。
    
    ```
    $ git status -s
     M welcome.txt
    ```


