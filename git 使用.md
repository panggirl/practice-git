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
###### 撤销回退3场景
场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

#### 添加远程库
场景是，你已经在本地创建了一个Git仓库后，又想在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步，这样，GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作，真是一举多得。
先登录 GitHub，在上面创建一个新的空仓库：learngit
GitHub的提示，
```
…or create a new repository on the command line
echo "# practicegit" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin git@github.com:panggirl/practicegit.git
git push -u origin master
```
在本地的learngit仓库下运行命令：
```
$ git remote add origin git@github.com:panggirl/learngit.git
```
请千万注意，把上面的panggirl替换成你自己的GitHub账户名，否则，你在本地关联的就是我的远程库，关联没有问题，但是你以后推送是推不上去的，因为你的SSH Key公钥不在我的账户列表中。
添加后，远程库的名字就是origin，这是Git默认的叫法，也可以改成别的，但是origin这个名字一看就知道是远程库。
下一步，就可以把本地库的所有内容推送到远程库上：
```
$ git push -u origin master
Counting objects: 71, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (53/53), done.
Writing objects: 100% (71/71), 122.48 KiB | 382.00 KiB/s, done.
Total 71 (delta 3), reused 0 (delta 0)
remote: Resolving deltas: 100% (3/3), done.
remote: 
remote: Create a pull request for 'master' on GitHub by visiting:
remote:      https://github.com/panggirl/practicegit/pull/new/master
remote: 
To github.com:panggirl/practicegit.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```
由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。
```
$ git push origin master
```

要关联一个远程空库，使用命令git remote add origin git@server-name:path/repo-name.git；
关联后，使用命令git push -u origin master第一次推送master分支的所有内容；
此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；

要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。
Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快
#### 创建与合并分支
创建dev分支，然后切换到dev分支：
```
$ git checkout -b dev
Switched to a new branch 'dev'
```
git checkout命令加上-b参数表示创建并切换，相当于以下两条命令：
```
$ git branch dev
$ git checkout dev
Switched to branch 'dev'
```
然后，用git branch命令查看当前分支：
```
$ git branch
* dev
  master
  ```
git branch命令会列出所有分支，当前分支前面会标一个*号。

在 `dev` 上对 readme.md 做修改然后
```
$ git commit -am "修改 readme.md"
[dev 7f992a0] 修改 readme.md
 1 file changed, 1 insertion(+)
```
现在，dev分支的工作完成，我们就可以切换回master分支：
```
$ git checkout master
Switched to branch 'master'
```
切换回master分支后，再查看一个readme.md文件，刚才添加的内容不见了！因为那个提交是在dev分支上，而master分支此刻的提交点并没有变
我们把dev分支的工作成果合并到master分支上：
```
$ git merge dev
Updating 29d9d5c..7f992a0
Fast-forward
 readme.md | 1 +
 1 file changed, 1 insertion(+)
```
git merge 命令用于合并指定分支到当前分支。合并后，再查看readme.md的内容，就可以看到，和dev分支的最新提交是完全一样的。
注意到上面的 Fast-forward 信息，Git告诉我们，这次合并是“快进模式”，也就是直接把master指向dev的当前提交，所以合并速度非常快。
合并完成后，就可以放心地删除dev分支了：
```
$ git branch -d dev
Deleted branch dev (was 7f992a0).
```
删除后，查看branch，就只剩下master分支了：
```
$ git branch
* master
```
因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在master分支上工作效果是一样的，但过程更安全。
```
Git鼓励大量使用分支：
查看分支：git branch
创建分支：git branch <name>
切换分支：git checkout <name>
创建+切换分支：git checkout -b <name>
合并某分支到当前分支：git merge <name>
删除分支：git branch -d <name>
```
#### 解决冲突
master 分支和 dev分支 针对同一个文件同一位置有修改，把 dev 分支合并到 master 时：
```
$ git merge dev
Auto-merging readme.md
CONFLICT (content): Merge conflict in readme.md
Automatic merge failed; fix conflicts and then commit the result.
```
冲突了！Git告诉我们，readme.md文件存在冲突，必须手动解决冲突后再提交。git status也可以告诉我们冲突的文件：
```
$ git status
On branch master
Your branch is up to date with 'origin/master'.

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

	both modified:   readme.md
```
  手动修改后：
```
 $ git add readme.md
 git commit -m "fix conflit"
[master 73e20cd] fix conflit
```
用带参数的git log也可以看到分支的合并情况：

```
$ git log --graph --pretty=oneline --abbrev-commit
*   7c79190 (HEAD -> master) git fic conflict
|\
| * e84b019 (dev) git conflict
* | da69c51 git world
|/
* 33751e8 (origin/master, origin/HEAD) git
*   73e20cd fix conflit
```
解决冲突
合并分支往往不是一帆风顺的。
准备新的feature1分支，继续我们的新分支开发：
``
$ git checkout -b feature1
Switched to a new branch 'feature1'
```
修改readme.md,在feature1分支上提交：
``
$ git add readme.txt
$ git commit -m "修改"
```
切换到master分支：
```
$ git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)
```
Git还会自动提示我们当前master分支比远程的master分支要超前1个提交。
在master分支上把readme.md 文件的同一位置做修改：
提交：
```
$ git add readme.txt 
$ git commit -m "master 修改"
```
现在，master分支和feature1分支各自都分别有新的提交，变成了这样：
```
git-br-feature1
```
这种情况下，Git无法执行“快速合并”，只能试图把各自的修改合并起来，但这种合并就可能会有冲突，我们试试看：
```
$ git merge feature1
Auto-merging readme.md
CONFLICT (content): Merge conflict in readme.md
Automatic merge failed; fix conflicts and then commit the result.
```
果然冲突了！Git告诉我们，readme.md文件存在冲突，必须手动解决冲突后再提交。git status也可以告诉我们冲突的文件：
```
$ git status
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

    both modified:   readme.md

no changes added to commit (use "git add" and/or "git commit -a")
```
我们修改下后保存,再提交：

```
$ git add readme.md
$ git commit -m "conflict fixed"
```
现在，用带参数的git log也可以看到分支的合并情况：

```
$ git log --graph --pretty=oneline --abbrev-commit
*   cf810e4 (HEAD -> master) conflict fixed
|\  
| * 14096d0 (feature1) AND simple
* | 5dc6824 & simple
|/  
* b17d20e branch test
* d46f35e (origin/master) remove test.txt
* b84166e add test.txt
* 519219b git tracks changes
* e43a48b understand how stage works
* 1094adb append GPL
* e475afc add distributed
* eaadf4e wrote a readme file
```
最后，删除feature1分支：

```
$ git branch -d feature1
Deleted branch feature1 (was 14096d0).
```
工作完成。

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。
解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。
用git log --graph命令可以看到分支合并图。
#### 分支管理策略
通常，合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。
如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

在 master 分支上准备合并dev分支，请注意--no-ff参数，表示禁用Fast forward：
```
$ git merge --no-ff -m "merge with no-ff" dev
Merge made by the 'recursive' strategy.
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```
因为本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去。
合并后，我们用git log(--abbrev-commit:缩写 commit id)看看分支历史：
```
$ git log --graph --pretty=oneline --abbrev-commit
*   e1e9c68 (HEAD -> master) merge with no-ff
|\  
| * f52c633 (dev) add merge
|/  
*   cf810e4 conflict fixed
...
```
在实际开发中，我们应该按照几个基本原则进行分支管理：
首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；
你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。
合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。

#### Bug分支
软件开发中，bug就像家常便饭一样。有了bug就需要修复，在Git中，由于分支是如此的强大，所以，每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。
当你接到一个修复一个代号101的bug的任务时，很自然地，你想创建一个分支issue-101来修复它，但是，等等，当前正在dev上进行的工作还没有提交：
```
$ git status
On branch dev
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   readme.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	temp.md
  ```
并不是不想提交，而是工作只进行到一半，还没法提交，预计完成还需1天时间。但是，必须在两个小时内修复该bug，怎么办？
幸好，Git还提供了一个`stash`功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：
```
$ git stash
Saved working directory and index state WIP on dev: e6738e4 merge with no-ff
```
现在，用git status查看工作区，就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复bug。
```
$ git status
On branch dev
Untracked files:
  (use "git add <file>..." to include in what will be committed)

	temp.md

nothing added to commit but untracked files present (use "git add" to track)
```
首先确定要在哪个分支上修复bug，假定需要在master分支上修复，就从master创建临时分支：
```
$ git checkout master
Switched to branch 'master'
$ git checkout -b issue-201
Switched to a new branch 'issue-201'
```
现在修复bug然后提交：
```
$ git commit -am "d"
[issue-201 511fbbb] d
 1 file changed, 2 insertions(+), 2 deletions(-)
hello kit
```
修复完成后，切换到master分支，并完成合并:
```
$ git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 4 commits.
  (use "git push" to publish your local commits)
$ git merge --no-ff -m "merge bug fix 201" issue-201
Merge made by the 'recursive' strategy.
 readme.md | 4 ++--
 1 file changed, 2 insertions(+), 2 deletions(-)
 ```
 现在，是时候接着回到dev分支干活了！
 ```
$ git checkout dev
Switched to branch 'dev'
```
刚才的工作现场存到哪去了？用git stash list命令看看：
```
$ git stash list
stash@{0}: WIP on dev: e6738e4 merge with no-ff
```
Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：
*一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；
*另一种方式是用git stash pop，恢复的同时把stash内容也删了
```
$ git stash pop 
On branch dev
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	temp.md

no changes added to commit (use "git add" and/or "git commit -a")
Dropped stash@{0} (96db7cc81dda8fcf6ded25ac1bf3128c4a462b8b)
```
再用git stash list查看，就看不到任何stash内容了：

$ git stash list
你可以多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash，用命令：
```
$ git stash apply stash@{0}
```
修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；
当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场。





 


