Welcome to My PMBar!
====================

----------

怎么配置本机的GIT，并上传文件
--------------

 1. 下载并安装GIT
> - [GIT下载地址][1] 

 2. 注册GIT的右键菜单 
> - 找到GIT安装目录，打开D:\Program Files\Git\git-cheetah 
> - 使用regsvr32 git_shell_ext.dll

 3. 配置Name和Email
> - 可以用命令，也可以在图形界面配置。 使用命令： 命令格式：git config --global user.name "your name"
> - git config --global user.email "your email address" 
> - 使用图形界面： 打开Git Gui,打开编辑》选项，在全局中配置用户名和email地址即可。并配置编码为UTF-8

 4. 生成Public RSA Key
> - 命令格式：ssh-keygen -C "your email address" -t rsa
> - 设置Public RSA Key的保存位置，直接回车采用默认地址；
> - 设置一个密码，并再次输入确认(这里不建议设置，方便本地使用)
> - Public RSA Key的保存路径：c:\users\username\.ssh\id_rsa.pub
> - 将Public Key告知Github
> - 请首先注册一个github账号，Home Page：https://github.com/ 。
> - 然后进入Account Settings页面，打开SSH Keys，点击“Add SSH Key”。
> - 打开c:\users\username\.ssh\id_rsa.pub，把里面的内容全部Copy到Key对应的输入框内，点击“Add Key”。

 5. 配置代理
> - 设置代理：git config --global http://proxy.yourname.com:8080
> - 如果需要用户名密码：git config –global http.proxy http://user:password@proxy.yourname.com:8080
> - 查看设置是否生效：git config --get --global http.proxy
> - 删除代理设置：git config --system (或 --global 或 --local) --unset http.proxy

 6. 异常处理
> - 提示unable to get local issuer certificate
> - git config --global http.sslVerify false

 7. 配置文件位置
> - 代理等配置：%HOMEPATH%\.gitconfig
> - 密钥：%HOMEPATH%\.ssh

  [1]: https://github.com/msysgit/msysgit/releases/
