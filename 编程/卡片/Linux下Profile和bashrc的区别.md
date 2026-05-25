---
tags:
  - Linux
up:
  - "[[Linux]]"
down:
relation:
---
[login shell和non-login shell的区别](https://blog.csdn.net/lws123253/article/details/89315218)

- Ubuntu 下打开一个新的终端默认是 non-login shell 模式，可以在终端设置中通过 Command -> Run command as a login shell 改为 login shell。
- `su <用户名> --login`：切换用户时使用 login 模式，否则为 non-login。
- `sudo su -`、`sudo su root --login`：以 login 模式切换到 root。
- login shell：通常是登录会话，会读取 profile 系列配置。
- non-login shell：通常是打开新的交互终端，会读取 bashrc 系列配置。

## 常见配置文件

1. `/etc/profile`：系统级 login shell 配置。
2. `~/.bash_profile`：用户级 login shell 配置；存在时通常优先于 `~/.profile`。
3. `~/.profile`：用户级 profile 配置，常用于设置 PATH。
4. `/etc/bash.bashrc`：系统级 interactive non-login shell 配置。
5. `~/.bashrc`：用户级 interactive non-login shell 配置，每次打开交互式 bash 通常都会执行。

## 区别

1. `~/.bash_profile` 是交互式 login 方式进入 bash 时运行的。
2. `~/.bashrc` 是交互式 non-login 方式进入 bash 时运行的。
