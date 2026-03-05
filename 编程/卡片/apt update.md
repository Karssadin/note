---
tags:
  - Linux
up:
  - "[[Linux]]"
down:
relation:
  - "[[GCC、G++、GDB、CMake安装]]"
---
APT（Advanced Package Tool）是 Debian/Ubuntu 系列 Linux 发行版的包管理工具。

## 常用命令

| 命令 | 说明 |
|------|------|
| `apt update` | 更新软件源索引（不安装任何东西，只刷新可用包列表） |
| `apt upgrade` | 升级所有已安装的包到最新版本 |
| `apt install <pkg>` | 安装指定软件包 |
| `apt remove <pkg>` | 卸载软件包（保留配置文件） |
| `apt purge <pkg>` | 卸载软件包并删除配置文件 |
| `apt autoremove` | 清理不再需要的依赖包 |
| `apt search <keyword>` | 搜索软件包 |
| `apt show <pkg>` | 查看包的详细信息 |
| `apt list --installed` | 列出所有已安装的包 |

## apt vs apt-get

- `apt` 是 `apt-get` + `apt-cache` 的整合命令（Ubuntu 16.04+），界面更友好（有进度条）
- `apt-get` 更适合脚本（输出格式稳定），`apt` 更适合交互使用
- 功能基本等价，新系统推荐用 `apt`

## 换源

将 `/etc/apt/sources.list` 中的默认源替换为国内镜像（清华、阿里、中科大）可大幅加速下载。换源后需执行 `apt update` 刷新索引。
