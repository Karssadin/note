---
tags:
  - Git
up:
  - "[[Git]]"
down:
relation:
  - "[[Git工作区模型]]"
---

# Git stash

`git stash` 用于临时保存工作区和暂存区改动，常用于切换分支前保存未完成工作。

## 常用命令

```bash
git stash push -m "message"
git stash list
git stash show -p stash@{0}
git stash pop
git stash apply stash@{0}
git stash drop stash@{0}
```

## 注意

1. `pop` 会应用并删除 stash，`apply` 只应用不删除。
2. 默认不保存未跟踪文件，可使用 `-u`。
3. stash 也可能产生冲突，需要像普通 merge 冲突一样解决。
