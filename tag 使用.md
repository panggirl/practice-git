# git Tag 使用
[标签管理-廖雪峰](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000
/0013762144381812a168659b3dd4610b4229d81de5056cc000)

* 命令 `git tag <tagname>`用于新建一个标签，默认为HEAD，也可以指定一个commit id；
* 命令`git tag -a <tagname> -m "blablabla..."`可以指定标签信息；
* 命令`git tag`可以查看所有标签;
* 命令`git show <tagname>`查看标签信息；

* 命令`git push origin <tagname>`推送某个标签到远程;
* 命令`$ git push origin --tags`一次性推送全部未推送到远程的本地标签；

* 命令`$ git tag -d <tagname>`删除本地标签;
删除远程标签：先删除本地标签，后远程删除
* 命令`$ git push origin :refs/tags/<tagname>`删除远程 tag；

* 命令`$ git fetch origin tag <tagname>`获取远程tag;




 

 
 
