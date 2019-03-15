#git 远程分支

#### git 推送本地分支到远程分支
推送本地分支`local_branch`到远程分支`remote_branch`并建立关联关系
  * 1.远程已有`remote_branch`分支并且已经关联本地分支`local_branch`且本地已经切换到`local_branch`
```
  git push
```
  * 2.远程已有`remote_branch`分支但未关联本地分支`local_branch`且本地已经切换到`local_branch`
```
   git push -u origin/remote_branch
```
  * 3.远程没有有`remote_branch`分支并，本地已经切换到`local_branch`
```
 git push origin local_branch:remote_branch
```
注：本地新建分支， push到远程服务器上之后，使用git pull或者git pull 拉取或提交数据时会报错，必须使用命令：git pull origin dev（指定远程分支）；如果想直接使用git pull或git push拉去提交数据就必须创建本地分支与远程分支的关联。

* 本地分支与远程分支关联
你想要做的是跟踪一个远程分支，这是通过git branch --set-upstream-to或更简单的git branch -u来完成的
git branch --set-upstream 本地新建分支名 origin/远程分支名
例：把本地dev分支和远程dev分支相关联
```
$ git branch --set-upstream-to origin/dev
Branch 'dev' set up to track remote branch 'dev' from 'origin'.
```
```
$ git branch --set-upstream-to=origin/dev
Branch 'dev' set up to track remote branch 'dev' from 'origin'.
```
```
$ git branch -u origin/dev
Branch 'dev' set up to track remote branch 'dev' from 'origin'.
```
注：[Difference between git branch --set-upstream-to vs git remote add origin](https://stackoverflow.com/questions/14169130/difference-between-git-branch-set-upstream-to-vs-git-remote-add-origin)
```
git remote add creates a remote, which is a shorthand name for another repository.  git branch --set-upstream-to sets a branch to be tracked by the branch in the remote repository specified.
git remote add创建一个远程，它是另一个存储库的简写名称。 git branch --set-upstream-to设置要由指定的远程存储库中的分支跟踪的分支。
```

### git push
`git push`的一般形式为 git push <远程主机名>:<本地分支名>  <远程分支名> ，例如 git push origin master：refs/for/master ，即是将本地的master分支推送到远程主机origin上的对应master分支， origin 是远程主机名，

`git push origin master` 如果远程分支被省略，如上则表示将本地分支推送到与之存在追踪关系的远程分支（通常两者同名），
如果该远程分支不存在，则会被新建。

如果省略本地分支名，则表示删除指定的远程分支，因为这等同于推送一个空的本地分支到远程分支。
```
$ git push origin :master
# 等同于
$ git push origin --delete master
```
上面命令表示删除origin主机的master分支.

如果当前分支与远程分支之间存在追踪关系，则本地分支和远程分支都可以省略。
```
$ git push origin
```
上面命令表示，将当前分支推送到origin主机的对应分支。

如果当前分支只有一个追踪分支，那么主机名都可以省略。
```
$ git push
```
如果当前分支与多个主机存在追踪关系，则可以使用-u选项指定一个默认主机，这样后面就可以不加任何参数使用git push。
$ git push -u origin master
上面命令将本地的master分支推送到origin主机，同时使用` -u `参数指定origin为默认主机，后面就可以不加任何参数使用git push了。

推送tag
```
$ git push origin tag_name
```

删除远程标签
```
$ git push origin :tag_name
```














