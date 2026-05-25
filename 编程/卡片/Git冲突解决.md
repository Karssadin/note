---
tags:
  - Git
up:
  - "[[Git]]"
down:
relation:
  - "[[merge与rebase]]"
  - "[[Git stash]]"
---

# Git冲突解决

Git 冲突通常发生在 merge、rebase、cherry-pick、stash pop 等操作中，表示同一位置的改动无法自动合并。

## 处理步骤

1. `git status` 查看冲突文件。
2. 打开文件，处理 `<<<<<<<`、`=======`、`>>>>>>>` 标记。
3. 运行测试或检查关键逻辑。
4. `git add <file>` 标记冲突已解决。
5. 根据当前操作执行 `git merge --continue`、`git rebase --continue` 或 `git cherry-pick --continue`。

## 放弃操作

```bash
git merge --abort
git rebase --abort
git cherry-pick --abort
```

## 注意

1. 不要盲目选择 ours/theirs，先理解两边改动语义。
2. 解决冲突后要重点检查删除、重命名和配置文件。
