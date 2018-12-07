## 分支

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
现在，master分支和feature1分支各自都分别有新的提交,这种情况下，Git无法执行“快速合并”，只能试图把各自的修改合并起来，
但这种合并就可能会有冲突，我们试试看：
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
* 用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；
* 用git stash pop，恢复的同时把stash内容也删了
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

软件开发中，总有无穷无尽的新的功能要不断添加进来。
添加一个新功能时，肯定不希望因为一些实验性质的代码，把主分支搞乱了，所以，每添加一个新功能，最好新建一个feature分支，在上面开发，
完成后，合并，最后，删除该feature分支。
于是准备开发：
```
$ git checkout -b feature-new
Switched to a new branch 'feature-new'
```
开发完毕 add commit,切回dev，准备合并：
``
$ git checkout dev
```
一切顺利的话，feature分支和bug分支是类似的，合并，然后删除。但是！就在此时，接到上级命令，因什么什么，新功能必须取消！
现在就是在不进行合并情况下删除新分支 feature-new：
```
$ git branch -d feature-new
error: The branch 'feature-new' is not fully merged.
If you are sure you want to delete it, run 'git branch -D feature-new'.
```
销毁失败。Git友情提醒，feature-new 分支还没有被合并，如果删除，将丢失掉修改，如果要强行删除，需要使用大写的-D参数。。

现在我们强行删除：
```
$ git branch -D feature-new
Deleted branch feature-new (was 719a1a4).
```
开发一个新feature，最好新建一个分支；如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。
#### 多人协作
当你从远程仓库克隆时，实际上Git自动把本地的master分支和远程的master分支对应起来了，并且，远程仓库的默认名称是origin。
要查看远程库的信息，用git remote：
```
$ git remote
origin
```
或者，用git remote -v显示更详细的信息：
```
$ git remote -v
origin	git@github.com:panggirl/learngit.git (fetch)
origin	git@github.com:panggirl/learngit.git (push)
```
上面显示了可以抓取和推送的origin的地址。如果没有推送权限，就看不到push的地址。
推送分支
推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定远程分支，这样，Git就会把该分支推送到远程库对应的远程分支上：
```
$ git push origin master
```
如果要推送其他分支，比如dev，就改成：
```
$ git push origin dev
```
但是，并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？
* master分支是主分支，因此要时刻与远程同步；
* dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
* bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
* feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。







