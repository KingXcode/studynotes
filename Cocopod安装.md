# CocoaPods安装
2018年9月3日

- 更换源
    
    ```
    gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/
    ```

- 查看是否更新成功，失败重新执行上面的更换

    ```
    gem sources -l
    ```

- 更新Ruby
    
    ```
    sudo gem update --system
    ```

- 安装cocopod,网上有说使用`sudo gem install cocoapods`的，实测会报错，这个是在早期的mac系统上使用的。
    
    ```
    sudo gem install -n /usr/local/bin cocoapods
    ```

- 查看是否安装成功

    ```
    pod --version
    ```

- 最后是安装仓库 （等待很漫长）

    ```
    pod setup
    ```


    
`~/.cocoapods`这个是安装仓库的目录

新建一个终端窗口,查看下载了多少

```
cd ~/.cocoapods
du -sh *
```


