# git 使用

在项目 project 目录下"git init"-初始化一个Git仓库，
```
$ git init
Initialized empty Git repository in /Users/dabo/WS/android/GitDemo/.git/
```
shell 返回的提示说明已经建立了一个 .git 隐藏目录，这个目录是 Git 来跟踪管理版本库的。这个目录是隐藏的，用 `ls -a`命令可以看到。
```
$ ls -a
./                 .gradle/           build.gradle       gradlew.bat
../                .idea/             gradle/            local.properties
.git/              GitDemo.iml        gradle.properties  settings.gradle
.gitignore         app/               gradlew*
```
然后添加文件到Git仓库，分两步：

使用命令git add <file>，注意，可反复多次使用，添加多个文件；
使用命令git commit -m <message>，完成。

`git add `这个是给项目制作个快照 snapshot（快照不等于记录在案，Git 管快照为称为stage（或者叫index）的暂存区），快照一般会暂时存储在一个临时存储区域中。
`git add . `这个是提交所有的。
`git commit`这个直接回车是会转到 vi 窗口，要求输入这个提交的版本和开发信息。commit 是把快照里登记的内容永久写入Git 仓库中。
`git commit`命令，-m 后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

然后在现在对原有的文件进行修改，此时开发者确认一下自己的修改：（working tree 工作区源代码）
`git diff --cached `这个命令是用来查看 index 和仓库之间代码区别的。当前只是在 working tree 里做了修改还没有报告给 index file，
所以此命令行输出空信息。
`git diff `这个命令是用来查看 working tree 和index file 之间的区别。
还可以用 `git status`来查看整体改动信息：
 * 提示“Changes not staged for commit:”: git 已经发现有修改但还没有  git add 的内容
 * 提示“changes to be committed”: git 已经发现已经 git add 但还没有 git commit 的内容
 * 提示“Untracked files:”: git 已经发现增加了新文件或 

`git checkout -- file` 对于工作区已修改的文件撤销修改
对已修改的文件：git add <file> --> git commit -m ,至此修改工作已完成且提交给了 git 。可以 git log 查看下。
对于每次add commit有一个偷懒的方法：git commit -a -m <message>可以直接提交所有修改，省去了git diff、git add 、git commit。但是
git commit -a 无法把新增的文件或文件夹添加到仓库，还是得 git add --> git commit。



