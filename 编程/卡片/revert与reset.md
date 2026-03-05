---
tags:
  - Git
up:
  - "[[编程/归档/八股文/Git]]"
down:
relation:
---

## 核心区别

| 维度 | `git revert` | `git reset` |
|------|-------------|-------------|
| 原理 | 创建一个新提交来"撤销"目标提交的修改 | 直接移动 HEAD 指针到指定提交 |
| 历史 | 保留完整历史（新增一个撤销提交） | 可能丢失历史（reset 之后的提交"消失"） |
| 公共分支 | **安全**（推荐） | **危险**（已 push 的提交不要 reset） |
| 用途 | 撤销某次错误提交 | 回退本地未 push 的提交 |

## git reset 三种模式

```
                          影响范围
                   HEAD    暂存区    工作区
--soft              移动     不变      不变     → 提交取消，修改保留在暂存区
--mixed（默认）      移动     重置      不变     → 提交取消，修改保留在工作区
--hard              移动     重置      重置     → 完全回退，修改全部丢失
```

```bash
git reset --soft HEAD~1    # 撤销最后一次提交，改动回到暂存区
git reset --mixed HEAD~1   # 撤销最后一次提交，改动回到工作区
git reset --hard HEAD~1    # 彻底丢弃最后一次提交及其改动
```

## git revert 用法

```bash
git revert <commit_id>     # 撤销指定提交（产生一个新的撤销提交）
git revert HEAD            # 撤销最新提交
git revert <id1>..<id2>    # 撤销一段范围的提交
```

如有冲突需手动解决后 `git revert --continue`。

## 选择建议

- 已 push 到远程的提交 → 用 `revert`
- 本地未 push 的提交 → 用 `reset`
- 不确定 → 用 `revert`（最安全）
- 误操作后恢复 → `git reflog` 找回 commit ID，再 `git reset --hard` 回去
