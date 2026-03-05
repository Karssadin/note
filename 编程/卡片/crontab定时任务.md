---
tags:
  - Linux
up:
  - "[[编程/归档/八股文/Linux]]"
down:
relation:
---

crontab 是 Linux 的定时任务调度工具，用于在指定时间自动执行命令或脚本。

## 基本操作

```bash
crontab -e          # 编辑当前用户的定时任务
crontab -l          # 查看当前用户的定时任务
crontab -r          # 删除当前用户的所有定时任务
sudo crontab -u user -e  # 编辑指定用户的定时任务
```

## 时间格式

```
┌──────── 分（0-59）
│ ┌────── 时（0-23）
│ │ ┌──── 日（1-31）
│ │ │ ┌── 月（1-12）
│ │ │ │ ┌ 周（0-7，0和7都是周日）
│ │ │ │ │
* * * * * command
```

## 常用示例

| 表达式 | 含义 |
|--------|------|
| `0 3 * * * /path/backup.sh` | 每天凌晨 3 点执行 |
| `*/5 * * * * cmd` | 每 5 分钟执行一次 |
| `0 0 * * 0 cmd` | 每周日零点执行 |
| `0 8-18 * * 1-5 cmd` | 工作日 8 点到 18 点，每整点执行 |
| `0 0 1 * * cmd` | 每月 1 号零点执行 |
| `@reboot cmd` | 系统启动时执行 |

## 日志与调试

- cron 日志：`/var/log/cron`（CentOS）或 `/var/log/syslog`（Ubuntu）中搜索 "CRON"
- 输出重定向：`* * * * * cmd >> /tmp/cron.log 2>&1`
- 环境变量：cron 执行环境与交互式 Shell 不同，建议在脚本中使用绝对路径或在 crontab 中设置 `PATH`
