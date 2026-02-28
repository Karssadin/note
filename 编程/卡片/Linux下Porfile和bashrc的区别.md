---
tags: 
up:
  - "[[Linux]]"
down: 
relation:
---
[login shell和non-login shell的区别](https://blog.csdn.net/lws123253/article/details/89315218)

- Ubuntu下打开一个新的终端默认是non-login shell模式，可以右键终端在设置中Command->Run command as a login shell就是每次设置为login shell了
- `su <用户名> --login`:切换用户时候使用login模式，不然为非login
- `sudo su -、sudo su root --login`以login模式切换到root
- login shell：使用密码登录
- /etc/profile、/etc/bash.bashrc、/.bash_profile、/.bashrc很容易混淆，他们之间有什么区别？它们的作用到底是什么？
    1. /etc/profile
    2. /.bash_profile，如果这个有的话，就不再执行下面的，这个里面会调用/.bashrc
    3. /.profile,会调用/.bashrc，并添加一些PATH
    4. ~/.bash_login
- su ksd --login
- non-login shell：打开一个新的终端

1. /etc/bash.bashrc
2. ~/.bashrc

- ~/.bashrc：每次打开shell都会执行
- `/etc/profile，/etc/bashrc`是系统全局环境变量设定
- `~/.profile，~/.bashrc`当前用户的私有环境变量设定

1. ~/.bash_profile 是交互式、login 方式进入 bash 运行的，意思是只有用户登录时才会生效。
2. ~/.bashrc 是交互式 non-login 方式进入 bash 运行的，用户不一定登录，只要以该用户身份运行命令行就会读取该文件。