# practice-github
练习 【GitHub 入门与实践】 
#### 前期准备

先安装 Git 参考[廖雪峰官网-安装Git](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137396287703354d8c6c01c904c7d9ff056ae23da865a000)，再设置姓名和邮箱账号地址安装完成后，在命令行输入：
```
$ git config --global user.name "Your Name"
$ git config --global user.email "your_email@example.com"
```
另外将 color.ui 设置为 auto 可以让命令的输出拥有更高的可读性：
```
$ git config --global color.ui auto
```
"~/.gitconfig"中会增加下面一行
```
[color]
  ui = auto
```
然后创建 GitHub 账号、设置头像(可以产生关注点或兴趣的)；设置[SSH Key](https://segmentfault.com/a/1190000002645623),其中`id_rsa`文件是私有密钥，`id_rsa.pub`是公开密钥，查看公开密钥的方法如下：
```
$ cat ~/.ssh/id_rsa.pub
ssh-rsa 公开密钥的内容 your_email@example.com
```
拿到 `id_rsa.pub`后添加到 GitHub ，今后就可以使用私有密钥进行认证了：点击账户设定按钮(Settings)选择 SSH Keys ,点击 Add SSH Key,出现输入 Title 和 Key 两个输入框，添加成功后，创建账户的邮箱会接收到一封提示“公开密钥添加完成”的邮件。完成这个设置后就可以用手中的私人密钥与GitHub进行认证和通讯了。让我们试试：
```
$ ssh -T git@github.com
Warning: Permanently added the RSA host key for IP address '13.229.188.59' to the list of known hosts.
Hi your_name! You've successfully authenticated, but GitHub does not provide shell access.
```
出现 `Hi your_name!...`结果即为成功（我的是已添加过所有会有一个 Warning）。
#### 创建仓库
点击右上角工具栏 “+”号选择 New repository，创建新的仓库。
* Repository name ：仓库的名称
* Description (optional) ：仓库的描述，非必填
* Public Private ，选 Public 创建公开仓库，仓库内所有内容都会被公开。选择 Private 可以创建非公开仓库，设置访问权限，但这个是收费的。
* Initialize this repository with a README ：在这个选项上打钩，GitHub 会自动初始化仓库并设置 README 文件。如果想向 GitHub 添加手中已有的Git 仓库时建议不要勾选，直接手动 push。
* Add .gitignore :下拉菜单非常方便，通过它可以在初始化时自动生成 .gitignore文件，省去了每次根据框架进行设置的麻烦。
* Add a license :下拉菜单可以选择添加的许可文件。如果这个仓库中包含的代码已经确定了许可协议那么就请在此选择。随后将自动生成包含许可证内容的 LICENSE 文件，来表明该仓库内容的许可协议。
输入选择都完成后，点击 Create repository 按钮，完成仓库的创建。
注：公开时的许可协议，即便GitHub上公开了源代码，也不代表作者放弃了著作权等权利。代码的权利持有人请选择合适的许可协议。实际使用中只需将 LICENSE 文件加入仓库，并在 README.md 文件中声明使用了何种许可协议即可。
