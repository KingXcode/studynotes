# Xcode 没有代码提示解决方案

## 方案一
1. 退出 Xcode
2. 重启电脑
3. 找到 这个 DerivedData 文件夹 删除 (路径: ~/Library/Developer/Xcode/DerivedData)
4. 删除这个 com.apple.dt.Xcode 文件 (路径: ~/Library/Caches/com.apple.dt.Xcode)
5. 运行 Xcode  就好了~~

## 方案二
1. 把.pch里的内容全部注释掉
2. clean掉项目里的内容
3. 把.pch里的注释去掉，编译。
4. 代码高亮，语法提示功能都回来了。


