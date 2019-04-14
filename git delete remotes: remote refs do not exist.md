#git delete remotes: remote refs do not exist

今天用git branch -a 命令看了一下，服务器上有一大堆的分支，大部分已经合并到master了。决定清理一下。

git push --delete origin myBranch

但是报错

error: unable to delete 'origin/myBranch': remote ref does not exist
既然remote端已经删掉，为什么用git branch -av还是能看到呢？ 其实我们看到的，只是前面用git fetch 保存到本地的缓存信息而已。
ok，we can simple do:

git fetch --prune origin
or just:

git fetch --p origin
这时候，再执行git branch -av ，已经看不到remote的myBranch这个分支了
