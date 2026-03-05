---
tags:
  - Git
up:
  - "[[编程/归档/八股文/Git]]"
down:
relation:
---

Git 采用四区模型管理代码版本：

## 四个区域

```
工作区 ──git add──▶ 暂存区 ──git commit──▶ 本地仓库 ──git push──▶ 远程仓库
  ◀──git checkout──   ◀──git reset──      ◀──git pull/fetch──
```

| 区域 | 位置 | 说明 |
|------|------|------|
| 工作区（Working Directory） | 本地文件系统 | 你正在编辑的文件 |
| 暂存区（Stage / Index） | `.git/index` | `git add` 后的快照，等待提交 |
| 本地仓库（Repository） | `.git` 目录 | `git commit` 后的完整历史 |
| 远程仓库（Remote） | GitHub / Gitee 等 | `git push` 后同步到服务器 |

## 数据流

- **`git add`**：工作区 → 暂存区（将修改标记为"待提交"）
- **`git commit`**：暂存区 → 本地仓库（创建新的提交快照）
- **`git push`**：本地仓库 → 远程仓库
- **`git pull`**：远程仓库 → 本地仓库 → 工作区（等于 fetch + merge）
- **`git fetch`**：远程仓库 → 本地仓库（不修改工作区）
- **`git checkout <file>`**：用暂存区/仓库版本覆盖工作区
- **`git reset`**：移动 HEAD 指针，根据模式影响暂存区和工作区

## 快照存储模型

Git 不是增量存储，而是**快照存储**：每次提交记录当前工作目录的完整状态，相同文件用指针复用（不重复存储）。提交之间通过父指针形成链表。
