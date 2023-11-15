## 查看远程库命令
```git remote -v```

```
origin  https://github.com/cxinping/PyQt5.git (fetch)
origin  https://github.com/cxinping/PyQt5.git (push)
```

## 修改远程库
### 方法一
```
git remote -v  #查看远端地址
git remote #查看远端仓库名
git remote set-url origin https://gitee.com/xx/xx.git (新地址)
```
### 方法二
```
git remote rm origin #删除远程的仓库
git remote add origin  https://gitee.com/xx/xx.git(新地址) #重新添加远程仓库
```

这种方法 类似于将本地项目推送到远程仓库，更简单

## 提交beta分支并推送到远程库
> 在git中，可利用checkout命令转换分支，该命令的作用就是切换分支或恢复工作树文件，语法为“git checkout 分支名”；当参数设置为“-b”时，可以在新分支创建的同时切换分支，语法为“git checkout -b 分支名”。

```git push --set-upstream origin beta```