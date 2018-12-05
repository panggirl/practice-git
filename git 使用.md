# git 使用
#### 创建版本库
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
#### 把文件添加到版本库
然后添加文件到Git仓库，分两步：
* 使用命令`git add <file>`: 用命令git add告诉Git，把文件添加到仓库.多次使用，添加多个文件；
* 使用命令`git commit -m <message>`: 用命令git commit告诉Git，把文件提交到仓库。message 用半角双引号包裹。
执行上面的命令，没有任何显示，这就对了，Unix的哲学是“没有消息就是好消息”，说明添加成功。
  
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
  
#### 版本回退
git status命令查看工作区的状态，告诉你有文件被修改过，用git diff可以查看修改内容。
git log 查看历史记录，显示从最近到最远的提交日志
```
$ git log 
commit ce3c8981352f023b8a3202e83e06374faed7d697 (HEAD -> master)
Author: cui <ipanggirl@163.com>
Date:   Wed Dec 5 13:37:08 2018 +0800

    修改 gitignore 文件

commit a61d181043b9fdfd46fe7bce6039f0c9915419ef
Author: cui <ipanggirl@163.com>
Date:   Wed Dec 5 11:03:14 2018 +0800

    add amd.txt

commit e9d1de0021e453cb1308f9320c711f60c71f5a08 (dev)
Author: cui <ipanggirl@163.com>
Date:   Wed Dec 5 11:00:35 2018 +0800

    first commit

```
输出信息太多，看得眼花缭乱的，可以试试加上--pretty=oneline参数:git log --pretty=oneline
```
$ git log --pretty=oneline
ce3c8981352f023b8a3202e83e06374faed7d697 (HEAD -> master) 修改 gitignore 文件
a61d181043b9fdfd46fe7bce6039f0c9915419ef add amd.txt
e9d1de0021e453cb1308f9320c711f60c71f5a08 (dev) first commit
```
看到的一大串类似ce3c898135...的是commit id（版本号）,十六进制表示的SHA1哈希数。
那退回上一个版本即“add amd.txt”版本：
首先，Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，也就是最新的提交1ce3c898...，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。用 git reset：
```
$ git reset --hard HEAD^
HEAD is now at a61d181 add amd.txt
```
回到某个版本还可以直接用 commit id（版本号没必要写全，前几位就可以了，Git会自动去找）不用 HEAD，现在想回退到版本“修改 gitignore 文件”处，
再看 log，发现没有版本“修改 gitignore 文件”处的commit id了，此时往上查看命令行记录可以看到，但关掉命令行再打开就没了，
```
$ git log --pretty=oneline
a61d181043b9fdfd46fe7bce6039f0c9915419ef (HEAD -> master) add amd.txt
e9d1de0021e453cb1308f9320c711f60c71f5a08 (dev) first commit
```
当你用$ git reset --hard HEAD^回退到“add amd.txt”版本时，再想恢复到“修改 gitignore 文件”，就必须找到“修改 gitignore 文件”的commit id。Git提供了一个命令git reflog用来记录你的每一次命令：
```
$ git reflog
a61d181 (HEAD -> master) HEAD@{0}: reset: moving to HEAD^
ce3c898 HEAD@{1}: reset: moving to ce3c89
e9d1de0 (dev) HEAD@{2}: reset: moving to e9d1de0
ce3c898 HEAD@{3}: commit: 修改 gitignore 文件
a61d181 (HEAD -> master) HEAD@{4}: merge amd: Fast-forward
e9d1de0 (dev) HEAD@{5}: checkout: moving from amd to master
a61d181 (HEAD -> master) HEAD@{6}: commit: add amd.txt
e9d1de0 (dev) HEAD@{7}: checkout: moving from dev to amd
e9d1de0 (dev) HEAD@{8}: checkout: moving from master to dev
e9d1de0 (dev) HEAD@{9}: commit (initial): first commit
```
从输出可知，“修改 gitignore 文件”的commit id是ce3c898:
```
$ git reset --hard ce3c898
HEAD is now at ce3c898 修改 gitignore 文件
```
回退版本大概路子是：
* HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间跳转，使用命令git reset --hard commit_id。

* 回退前，用git log可以查看提交历史，以便确定要回退到哪个版本。

* 要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本
##### 撤销修改
Android studio 3.2版本中的返回 紫色的返回（Revert ）按钮， 即 git 撤销修改按钮，命令行是 `git checkout -- file`可以丢弃工作区的修改：
```
git checkout -- app/src/main/java/com/dabo/gitdemo/MainActivity.java
```
命令git checkout -- file 意思就是，把 file 文件在工作区的修改全部撤销，这里有两种情况：

一种是 file 自修改后还没有 add 即被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是 file 已经添加 add 到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

可以不理会上面说的两种情况，总之，就是让这个文件撤销已做的修改前的状态，不用关心最近一次 git 操作的状态。
git checkout -- file命令中的`--`很重要，没有`--`，就变成了“切换到另一个分支”的命令.

但下面这个撤销命令就关心最近一次 git 操作状态，即：`git reset HEAD <file>`可以把暂存区的修改撤销掉（unstage），重新放回工作区.
情况就是已经 add 了但还没有 commit 时：
```
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   app/src/main/java/com/dabo/gitdemo/MainActivity.java
 
```
git reset命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用HEAD时，表示最新的版本
```
$ git reset HEAD app/src/main/java/com/dabo/gitdemo/MainActivity.java
Unstaged changes after reset:
M       app/src/main/java/com/dabo/gitdemo/MainActivity.java
```
再次 git status ,现在暂存区是干净的，工作区有修改：
```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   app/src/main/java/com/dabo/gitdemo/MainActivity.java

no changes added to commit (use "git add" and/or "git commit -a")
```
###### 
场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。






 


