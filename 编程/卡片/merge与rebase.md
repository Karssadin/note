---
tags:
  - Git
up:
  - "[[编程/归档/八股文/Git]]"
down:
relation:
---

## 区别

| 维度 | merge | rebase |
|------|-------|--------|
| 原理 | 创建新的合并提交，包含两个父指针 | 将当前分支的提交"重放"到目标分支顶端 |
| 历史 | 保留分支历史（有分叉） | 线性化历史（无分叉） |
| 合并提交 | 产生一个 merge commit | 不产生额外提交 |
| 适用 | 公共分支（main/develop）合并 feature | 本地分支整理（保持历史干净） |
| 冲突 | 一次性解决所有冲突 | 可能需要逐个提交解决冲突 |

## 使用建议

- **公共分支上只用 merge，不用 rebase**：rebase 会改写提交历史，如果别人已经基于这些提交工作，会导致历史混乱
- **个人 feature 分支用 rebase**：合入 main 前先 `git rebase main`，让历史线性
- **合并到主分支时用 merge**：`git merge --no-ff feature` 保留 merge commit 记录

## 交互式 rebase

`git rebase -i <commit_id>` 可以对多个提交进行编辑：

| 操作 | 说明 |
|------|------|
| pick | 保留该提交 |
| squash | 合并到上一个提交 |
| edit | 修改该提交 |
| drop | 删除该提交 |
| reword | 修改提交信息 |

常见场景：将多个小提交压缩为一个再合入主分支。

## merge --squash

```bash
git merge --squash feature   # 将 feature 的所有修改合并但不提交
git commit -m "feature完成"  # 手动提交为一个提交
```

效果类似 rebase + squash，但不改写 feature 分支的历史。
