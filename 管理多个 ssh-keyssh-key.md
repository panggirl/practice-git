### 管理多个 ssh-key
默认会使用的 private key 为 `id_sra`,每次生成 ssh-key 时，会覆盖原有的key。
由于公司使用的 gitlab  和 自己的 GitHub 都要使用，所以就得管理多个 ssh-key。
在生成ssh-key时，会让输入一个key名，默认是 id_rsa。需要管理多个key的情况下，
建议这个key名是自定义，后面跟着域名的key以方便管理和查看。因此key的名字可以以这种方式命名： id_rsa_hostname

```
ssh-keygen -t rsa -C "your_name@email.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/Users//.ssh/id_rsa):id_rsa_hostname
```
或者 `ssh-keygen -t rsa -C "fupaizai@163.com" -f id_rsa_fupai`
```
[dabo@1024 09:49:47 ~/.ssh]
$ ssh-keygen -t rsa -C "fupaizai@163.com" -f id_rsa_fupai
Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
```
##### 生成ssh-key
生成ssh-key的时候在 .ssh文件目录下可以看到刚才以 id_rsa_hostname命名的两个文件 —— id_rsa_hostname和 id_rsa_hostname.pub。
这两个文件一个是私钥一个是公钥

##### 配置ssh-key
打开或者查看公钥文件` cat ~/.ssh/id_rsa_hostname.pub`。复制里面的内容粘贴到需要设置的域名中，如：GitHub，
在GitHub设置中添加ssh。将内容粘贴到SSH keys里面。
#####  配置多个ssh-key
[linux/mac vi命令详解](https://blog.csdn.net/youngkingyj/article/details/22713965)

在 .ssh文件目录下创建一个config文件,编辑文件：

```
  cd .ssh
  vi config
```

将配置的内容添加进去，以下是需要添加的内容：

```
  # github
  # 对应 hostname的别名 
  Host github.com
  # 这个是域名地址
  HostName github.com
  # github对应的email或者用户名
  User name
  PreferredAuthentications publickey
  # github对应的私钥地址
  IdentityFile ~/.ssh/id_rsa_github

  # your company: cpy.com
  Host cpy.com
  HostName cpy.com
  User your_name
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/id_rsa_cpy
```
关于 Host 和 Hostname 的对应关系:
 配置一个Host，Host为HostName的别名，Host的名字可以取为自己喜欢的名字，不过这个会影响git相关命令，例如：Host mygithub 这样定义的话，
 命令下，即git@后面紧跟的名字改为mygithub，例如：git clone git@mygithub:your_github_name/your_project_name.git,如果 Hostname 是域名则最好保持一致。但是这里有两个诀窍：
1. 如果同一域名下有两个不同的配置怎么办？以 Github 为例，如果我有两个账户，一个个人的，一个组织的，并且要使用不同的秘钥，
那么我可以这么写：这里 Host 后面对应的是 Github 的两个用户名，也就是 github.com/name1 和 github.com/name2
2. 如果域名是数字 IP，是否可以简化呢？
Host 可以帮助你把对应的 IP 变成好记的名字。比如说我在公司内部配置了 Git Server（基于 gitolite 或 Gitlab 或任何工具），
正常的访问地址是：git://xxx.xxx.xxx.xxx:repo.git，如下的配置则可以帮你把它简化成：git.visionet:repo.git
 
##### 测试 ssh 
```
ssh -T git@github.com
```
你会得到如下提示，表示这个ssh公钥已经获得了权限
```
Hi USERNAME! You've successfully authenticated, but github does not provide shell access.
```
如果报 `ssh: connect to host gitlab.your_compay_host_name.com port 22: Connection refused` 时,是由于你公司的gitlab项目地址
后面有配端口号，这时在 ~/.ssh/config 里添加端口号配置：
```
# your company: cpy.com
  Host cpy.com
  HostName cpy.com
  User your_name
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/id_rsa_cpy
  Port 443
```


连接远程仓库
```
git remote add origin 仓库地址
```

查看远程连接

```git remote -v```

git取消与远程仓库的连接
```git remote remove origin```

