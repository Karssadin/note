---
tags:
  - Linux
up:
  - "[[Linux]]"
down:
relation:
  - "[[Linux指令与选项 格式]]"
  - "[[GCC、G++、GDB、CMake安装]]"
  - "[[Shell编程基础]]"
---

## 文件与目录

| 命令 | 说明 | 常用示例 |
|------|------|---------|
| `pwd` | 打印当前工作目录 | `pwd` |
| `ls` | 列出目录内容 | `ls -lah` |
| `cd` | 切换目录 | `cd ~`、`cd ..`、`cd -` |
| `mkdir` | 创建目录 | `mkdir -p a/b/c` |
| `touch` | 创建空文件/更新时间戳 | `touch file.txt` |
| `rm` | 删除文件/目录 | `rm -rf dir/` |
| `cp` | 复制 | `cp -r src/ dst/` |
| `mv` | 移动/重命名 | `mv old.txt new.txt` |
| `find` | 查找文件 | `find /etc -name "*.conf"` |
| `tree` | 树状显示目录结构 | `tree -L 2` |

**ls 选项说明：**
- `-l`：详细列表（权限、大小、日期）
- `-a`：显示隐藏文件（含 `.` 和 `..`）
- `-h`：人类可读格式（KB/MB/GB）

## 文本处理

| 命令 | 说明 | 常用示例 |
|------|------|---------|
| `cat` | 显示文件内容 | `cat -n file.txt`（显示行号） |
| `less` / `more` | 分页查看 | `less log.txt`（按 q 退出） |
| `head` | 查看前 N 行 | `head -n 20 file.txt` |
| `tail` | 查看尾部/实时追踪 | `tail -f /var/log/syslog` |
| `grep` | 文本搜索 | `grep -r "error" /var/log/` |
| `sed` | 流式文本编辑 | `sed 's/old/new/g' file.txt` |
| `awk` | 列处理工具 | `awk '{print $1}' file.txt` |
| `wc` | 统计行数/字数/字节 | `wc -l file.txt` |
| `sort` | 排序 | `sort -n -k2 file.txt` |
| `uniq` | 去重（配合 sort） | `sort \| uniq -c` |
| `cut` | 按列切割 | `cut -d: -f1 /etc/passwd` |

**grep 常用选项：**
```bash
grep -r "keyword" /path/    # 递归搜索
grep -n "keyword" file.txt  # 显示行号
grep -i "keyword" file.txt  # 忽略大小写
grep -v "keyword" file.txt  # 反向匹配（不含 keyword）
grep -l "keyword" *.txt     # 只显示文件名
grep -E "regex" file.txt    # 使用扩展正则
```

**awk 基本用法：**
```bash
awk '{print $1, $3}' file.txt          # 打印第1、3列
awk -F: '{print $1}' /etc/passwd       # 指定分隔符
awk '/pattern/ {print $0}' file.txt    # 匹配行
awk '{sum += $1} END {print sum}' f    # 累加求和
```

**sed 基本用法：**
```bash
sed 's/old/new/g' file.txt             # 全局替换（不修改文件）
sed -i 's/old/new/g' file.txt         # 直接修改文件
sed -n '5,10p' file.txt               # 打印第5-10行
sed '/pattern/d' file.txt             # 删除匹配行
```

## 权限管理

```bash
chmod 755 file.sh              # 八进制权限（rwxr-xr-x）
chmod +x script.sh             # 添加执行权限
chown user:group file          # 修改文件所有者
ls -l                          # 查看权限（drwxr-xr-x）
```

权限数字：4=读(r)，2=写(w)，1=执行(x)，常用：777/755/644

## 进程管理

```bash
ps aux                         # 查看所有进程
ps -ef | grep nginx            # 查找特定进程
top                            # 实时进程监控（q 退出）
htop                           # 增强版 top（需安装）
kill -9 PID                    # 强制终止进程
killall nginx                  # 按名字终止
pgrep nginx                    # 查找进程 PID
nohup cmd &                    # 后台运行，忽略 SIGHUP
```

## 网络工具

```bash
ifconfig / ip addr             # 查看网络接口
ping 8.8.8.8                   # 测试连通性
traceroute 8.8.8.8             # 路由追踪
netstat -tulnp                 # 查看监听端口（需 net-tools）
ss -tulnp                      # netstat 替代（更快）
curl -X GET http://url         # 发送 HTTP 请求
wget http://url/file           # 下载文件
scp user@host:/path/file .     # 远程复制文件
ssh user@host                  # SSH 登录
```

## 磁盘与内存

```bash
df -h                          # 查看磁盘使用情况
du -sh /path/                  # 查看目录大小
du -sh * | sort -h             # 按大小排序
free -h                        # 查看内存使用
lsblk                          # 列出块设备
```

## 压缩与解压

```bash
tar -czvf archive.tar.gz dir/  # 压缩目录为 tar.gz
tar -xzvf archive.tar.gz       # 解压 tar.gz
tar -cjvf archive.tar.bz2 dir/ # 压缩为 bz2
gzip file.txt                  # 压缩（生成 file.txt.gz）
gunzip file.txt.gz             # 解压
zip -r archive.zip dir/        # 压缩为 zip
unzip archive.zip              # 解压 zip
```

## 系统信息

```bash
uname -a                       # 系统内核信息
hostname                       # 主机名
whoami                         # 当前用户
date                           # 当前日期时间
uptime                         # 系统运行时长
lscpu                          # CPU 信息
cat /proc/meminfo              # 内存信息
```

## 重定向与管道

```bash
cmd > file         # stdout 写入文件（覆盖）
cmd >> file        # stdout 追加到文件
cmd 2> err.log     # stderr 写入文件
cmd > file 2>&1    # stdout+stderr 都写入文件
cmd1 | cmd2        # 管道：cmd1 输出作为 cmd2 输入
ctrl + l           # 清屏
```

## 常用快捷键

| 快捷键 | 功能 |
|--------|------|
| `Ctrl+C` | 终止当前命令 |
| `Ctrl+Z` | 挂起到后台 |
| `Ctrl+D` | EOF / 退出 Shell |
| `Ctrl+L` | 清屏 |
| `!!` | 重复上一条命令 |
| `!n` | 执行历史第 n 条命令 |
| `history` | 查看历史命令 |
| `tab` | 自动补全 |

## 关机与重启

```bash
shutdown -h now    # 立即关机
shutdown -r now    # 立即重启
reboot             # 重启
poweroff           # 关机
```
