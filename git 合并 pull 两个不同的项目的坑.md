
# git 合并 pull 两个不同的项目的坑
在GitHub上创建了仓库且创建了 README 和 .gitignore 文件，在本地的仓库也有 .gitignore 文件，然后想把这本地仓库同GitHub上关联起来，具体坑见下面

### 坑1 fatal: remote origin already exists.
```
[dabo@1024 16:18:30 ~/Android/android_project/完整项目学习/PracticeAnimation]
$ git  remote add origin git@github.com:panggirl/practice-animation.git
fatal: remote origin already exists.
```

github常见操作和常见错误！错误提示：`fatal: remote origin already exists.`
解决办法如下：
```
1、先输入$ git remote rm origin
2、再输入$ git  remote add origin git@github.com:panggirl/practice-animation.git 就不会报错了！
```

### 坑2  git无法pull仓库refusing to merge unrelated histories
就在GitHub上创建了仓库且创建了 README 和 .gitignore 文件，在本地的仓库也有 .gitignore 文件，然后想把这本地仓库同GitHub上关联起来，具体操作如下：
```
[dabo@1024 17:21:18 ~/Android/android_project/PracticeAnimation]
$ git remote add origin git@github.com:panggirl/practice-animation.git

[dabo@1024 17:21:20 ~/Android/android_project/PracticeAnimation]
$ git push -u origin master
To github.com:panggirl/practice-animation.git
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'git@github.com:panggirl/practice-animation.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

[dabo@1024 17:22:35 ~/Android/android_project/PracticeAnimation]
$ git pull origin master
From github.com:panggirl/practice-animation
 * branch            master     -> FETCH_HEAD
 * [new branch]      master     -> origin/master
fatal: refusing to merge unrelated histories

```
这种应该属于合并pull两个不同的项目，出现的问题
如果合并了两个不同的开始提交的仓库，在新的 git 会发现这两个仓库可能不是同一个，为了防止开发者上传错误，于是就给下面的提示
```
fatal: refusing to merge unrelated histories
```
如我在Github新建一个仓库，写了License，然后把本地一个写了很久仓库上传。这时会发现 github 的仓库和本地的没有一个共同的 commit
所以 git 不让提交，认为是写错了 origin ，如果开发者确定是这个 origin 就可以使用 `--allow-unrelated-histories` 告诉 git 自己确定

遇到无法提交的问题，一般先pull 也就是使用 `git pull origin master` 这里的 origin 就是仓库，而 master 就是需要上传的分支，因为两个
仓库不同，发现 git 输出` refusing to merge unrelated histories `无法 pull 内容

因为他们是两个不同的项目，要把两个不同的项目合并，git需要添加一句代码，在 git pull 之后，这句代码是在git 2.9.2版本发生的，最新的版本需
要添加 `--allow-unrelated-histories` 告诉 git 允许不相关历史合并

假如我们的源是origin，分支是master，那么我们需要这样写`git pull origin master --allow-unrelated-histories` 如果有设置了默认上传分
支就可以用下面代码
```
git pull --allow-unrelated-histories
```
