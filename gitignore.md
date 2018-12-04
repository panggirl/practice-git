#### .gitignore文件问题
github上.gitignore模板合集，里面有各种[.gitignore ](https://github.com/github/gitignore)

有时我们发现添加.gitignore文件后并没有忽略我们想要忽略的文件，解决方法就是清除一下缓存，原因gitignore对已经追踪(track)的文件无效，
清除缓存后文件将以未追踪的形式出现.然后再重新添加提交一下,.gitignore文件里的规则就可以起作用了
```
git rm -r --cached .
git add .
git commit -m 'update .gitignore'
```
#### 相关过滤规则举例说明：
```
#：注释符号，自动被Git忽略
*.iml：过滤所有的.iml后缀的文件*
.gradle/：过滤掉.gradle文件夹
/gradle  :过滤掉 gradle 文件夹
local.properties：过滤掉local.properties文件
```
