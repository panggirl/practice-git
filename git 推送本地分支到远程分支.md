# git 推送本地分支到远程分支
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
