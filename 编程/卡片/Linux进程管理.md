---
tags:
  - Linux
up:
  - "[[编程/归档/八股文/Linux]]"
down:
relation:
  - "[[fork]]"
  - "[[死锁的概念及产生原因]]"
---

## 查看进程

| 命令 | 说明 |
|------|------|
| `ps aux` | 列出所有进程（BSD 风格） |
| `ps -ef` | 列出所有进程（POSIX 风格） |
| `top` | 实时查看进程资源占用（CPU/内存），按 `M` 按内存排序，`P` 按 CPU 排序 |
| `htop` | `top` 的增强版，支持鼠标、彩色显示 |
| `pstree` | 树形显示进程关系 |
| `pidof <name>` | 根据进程名查 PID |

## 进程控制

| 命令 | 说明 |
|------|------|
| `kill <PID>` | 发送 SIGTERM（可被捕获） |
| `kill -9 <PID>` | 发送 SIGKILL（强制杀死，不可捕获） |
| `killall <name>` | 按进程名杀死所有匹配进程 |
| `nohup cmd &` | 后台运行，关闭终端不中断 |
| `cmd &` | 后台运行（关终端会中断） |
| `jobs` | 查看当前 Shell 的后台任务 |
| `fg %N` | 将任务 N 切回前台 |
| `bg %N` | 将暂停的任务 N 在后台继续运行 |
| `Ctrl+Z` | 暂停当前前台进程 |
| `Ctrl+C` | 发送 SIGINT 终止当前进程 |

## systemd 服务管理

systemd 是现代 Linux 的初始化系统和服务管理器（替代 SysV init）。

```bash
systemctl start <service>      # 启动服务
systemctl stop <service>       # 停止服务
systemctl restart <service>    # 重启服务
systemctl enable <service>     # 开机自启
systemctl disable <service>    # 取消开机自启
systemctl status <service>     # 查看服务状态
systemctl list-units --type=service  # 列出所有服务
```

### 服务单元文件

位于 `/etc/systemd/system/` 或 `/usr/lib/systemd/system/`：

```ini
[Unit]
Description=My Service
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/myapp
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

## 进程优先级

```bash
nice -n 10 cmd                 # 以优先级 10 启动（范围 -20~19，越小优先级越高）
renice -n 5 -p <PID>           # 修改运行中进程的优先级
```
