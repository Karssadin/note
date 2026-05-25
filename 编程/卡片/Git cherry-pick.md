---
tags:
  - Git
up:
  - "[[Git]]"
down:
relation:
  - "[[merge与rebase]]"
  - "[[revert与reset]]"
---

# Git cherry-pick

`git cherry-pick` 用于把其他分支上的某个提交复制到当前分支，生成一个新的提交。

## 常用命令

```bash
git cherry-pick <commit>
git cherry-pick <start>^..<end>
git cherry-pick --continue
git cherry-pick --abort
```

## 适用场景

1. 只想引入某个 bugfix，不合并整条分支。
2. 把 hotfix 同步到多个发布分支。
3. 从实验分支挑选部分提交。

## 注意

1. cherry-pick 会生成新提交，提交 hash 会变化。
2. 遇到冲突后解决冲突并执行 `--continue`。
3. 大量 cherry-pick 可能让历史难以追踪，优先考虑正常 merge/rebase。
