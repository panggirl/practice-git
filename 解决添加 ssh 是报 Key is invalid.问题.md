Git学习碰到的问题
连接 github 时
```
$ ssh-keygen -t rsa -C "abcd@efgh.com" //你的邮箱
然后将 key 粘贴到 github 设置里的 SSH keys 报出如下错误：
Key is invalid. It must begin with 'ssh-ed25519', 'ssh-rsa', 'ssh-dss', 'ecdsa-sha2-nistp256', 
'ecdsa-sha2-nistp384', or 'ecdsa-sha2-nistp521'. Check that you're copying the public half of the key
```
解决：ssh key 格式问题。在执行 ssh-keygen 命令后会提示输入保存 key 的文件，注意打开的是对应的 .pub 格式的文件，
用 sublime 等软件打开，注意保持其原来的格式，然后直接贴到 github上即可。（.pub 里的格式为："ssh-rsa 空格 key值 空格 邮箱")
