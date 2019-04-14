# 重命名分支
1 、远程分支重命名
其实就是先删除远程分支，然后重命名本地分支，再重新提交一个远程分支
2、本地 branch 重命名 foo => bar

* 切到分支 foo
```
git branch -m bar
```
* 直接重命名
```
git branch -m foo bar
```
