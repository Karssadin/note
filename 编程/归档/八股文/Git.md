---
tags:
  - 编程
  - 八股文
  - Git
up:
down:
  - "[[Git工作区模型]]"
  - "[[merge与rebase]]"
  - "[[revert与reset]]"
relation:
  - "[[Linux]]"
---
- [[#基础配置|基础配置]]
- [[#核心概念|核心概念]]（详见 [[Git工作区模型]]）
- [[#commit|commit]]
- [[#branch|branch]]
- [[#checkout 和 switch|checkout 和 switch]]
- [[#查看状态与历史|查看状态与历史]]
- [[#暂存与恢复|暂存与恢复]]
- [[#远程操作|远程操作]]
- [[#merge 与 rebase|merge 与 rebase]]（详见 [[merge与rebase]]）
- [[#冲突解决|冲突解决]]
- [[#cherry-pick|cherry-pick]]
- [[#revert 与 reset 区别|revert 与 reset 区别]]（详见 [[revert与reset]]）
- [[#标签管理|标签管理]]
- [[#.gitignore|.gitignore]]
- [[#其他命令|其他命令]]

## 基础配置

```shell
git config --global user.name "Your Name"
git config --global user.email "youremail@example.com"
git config --list    # 查看所有配置
```

## 核心概念

Git 采用**快照存储模型**，每次提交记录当前工作目录的完整快照（差异打包优化），并通过父指针形成提交链。

- **工作区**（Working Directory）：本地文件系统
- **暂存区**（Stage / Index）：`git add` 后的中间区域
- **本地仓库**（Repository）：`.git` 目录，存储所有提交历史
- **远程仓库**（Remote）：GitHub / Gitee 等远程服务器

**HEAD 指针**：指向当前分支最新提交，通过 `HEAD~` / `HEAD^` 进行相对引用。
- `HEAD~2`：上上个提交
- `HEAD^2`：第二个父节点（merge 时产生）
- `git reflog`：查看 HEAD 指针的历史变动

## commit

- 提交对象包含：提交信息、作者、时间戳、父指针、文件快照
- 提交后当前分支指针自动前进

```shell
git commit -m "message"          # 带信息提交
git commit --amend               # 修改最近一次提交
git commit --squash <commit_id>  # 压缩合并多个提交
```

## branch

分支本质是指向某个提交的可变指针，创建分支只是新建指针，不复制数据。

```shell
git branch <name>                # 创建分支
git branch <name> <commit_id>   # 基于指定提交创建分支
git branch -d <name>             # 删除分支
git branch -D <name>             # 强制删除
git branch -m <new_name>         # 重命名当前分支
```

## checkout 和 switch

切换分支时工作目录文件替换为目标提交的快照，HEAD 指向新分支。

```shell
git checkout <branch>                    # 切换分支
git checkout -b <branch> [commit_id]     # 创建并切换
git checkout <commit_id> <file_path>     # 恢复指定文件到指定版本

git switch <branch>                      # 切换分支（推荐）
git switch -c <branch> [commit_id]       # 创建并切换（推荐）
```

推荐使用 `switch` 替代 `checkout` 进行分支切换，`checkout` 保留用于检出文件。

## 查看状态与历史

```shell
git status                        # 工作区与暂存区状态
git log --oneline --graph         # 简洁图形化提交历史
git log --pretty=oneline
git diff                          # 工作区 vs 暂存区
git diff --cached                 # 暂存区 vs 仓库
git diff HEAD -- <file>           # 工作区 vs 仓库最新版本
git diff <id1> <id2>              # 两个提交之间的差异
```

## 暂存与恢复

### add / restore
```shell
git add <file>                    # 工作区 → 暂存区
git restore <file>                # 恢复工作区文件到暂存区版本
git restore --staged <file>       # 暂存区文件恢复到 HEAD 版本
```

### reset
```shell
git reset <file>                  # 从暂存区移除（保留工作区）
git reset --soft <commit_id>      # 仅移动 HEAD，暂存区和工作区不变
git reset --mixed <commit_id>     # 移动 HEAD + 重置暂存区（默认）
git reset --hard <commit_id>      # 移动 HEAD + 重置暂存区 + 重置工作区
```

### stash
```shell
git stash                         # 保存当前工作区和暂存区状态
git stash list                    # 查看 stash 列表
git stash pop                     # 恢复并删除最近的 stash
git stash apply stash@{0}         # 恢复指定 stash（不删除）
git stash drop                    # 删除 stash
```

典型场景：开发中需要紧急修 bug → stash 保存 → 切换分支修复 → 切回 → stash pop。

## 远程操作

```shell
git remote add origin <url>       # 关联远程仓库
git remote -v                     # 查看远程仓库详情
git remote rm origin              # 删除远程关联

git clone <url>                   # 克隆远程仓库
git fetch                         # 拉取远程更新（不合并）
git pull                          # fetch + merge
git push -u origin master         # 首次推送（-u 设置上游）
git push origin <branch>          # 后续推送
```

### 远程分支更新
- 未指定 `--single-branch` 时：`git fetch` 后直接 `checkout` 切换
- 指定了 `--single-branch` 时：
  ```shell
  git remote set-branches --add origin <remote-branch>
  git fetch origin <remote-branch>:<local-branch>
  git branch --set-upstream-to=origin/<remote-branch> <local-branch>
  ```

## merge 与 rebase

```shell
git merge <branch>                # 合并（产生合并提交，保留分支历史）
git merge --squash <branch>       # 合并但不产生合并提交
git rebase <branch>               # 变基（线性化历史，更整洁）
git rebase -i <commit_id>         # 交互式变基（压缩、修改、删除提交）
```

- `merge` 创建新的合并提交，包含两个父指针
- `rebase` 将当前分支的提交"移动"到目标分支顶端，历史更干净
- 公共分支（如 main）上不要 rebase，只在本地/feature 分支使用

## 冲突解决

当 merge 或 rebase 产生冲突时：

1. Git 在冲突文件中标记 `<<<<<<<`、`=======`、`>>>>>>>`
2. 手动编辑文件，保留正确内容
3. `git add <file>` 标记为已解决
4. `git commit`（merge）或 `git rebase --continue`（rebase）

```shell
git merge --abort                 # 放弃 merge
git rebase --abort                # 放弃 rebase
```

## cherry-pick

将另一个分支的指定提交复制到当前分支（不是整个分支的合并）。

```shell
git cherry-pick <commit_id>       # 复制单个提交
git cherry-pick <id1> <id2>       # 复制多个提交
git cherry-pick <id1>..<id2>      # 复制范围（不含 id1）
```

如有冲突需手动解决后 `git cherry-pick --continue`。

## revert 与 reset 区别

| 特性 | `git revert` | `git reset` |
|------|-------------|-------------|
| 原理 | 创建新提交来"撤销"旧提交 | 直接移动 HEAD 指针 |
| 历史 | 保留完整历史 | 可能丢失历史 |
| 公共分支 | 安全（推荐） | 危险（不要在已 push 的提交上用） |

```shell
git revert <commit_id>            # 安全撤销某次提交
git revert HEAD                   # 撤销最新提交
```

## 标签管理

```shell
git tag <tag_name>                          # 创建轻量标签
git tag -a <tag_name> -m "message"          # 创建带注释的标签
git tag                                     # 查看所有标签
git push origin <tag_name>                  # 推送标签到远程
git push origin --tags                      # 推送所有标签
git tag -d <tag_name>                       # 删除本地标签
git push origin :refs/tags/<tag_name>       # 删除远程标签
```

## .gitignore

在仓库根目录创建 `.gitignore` 文件，声明不需要版本控制的文件：

```gitignore
# 编译产物
*.o
*.exe
build/

# IDE 配置
.vscode/
.idea/
*.swp

# 系统文件
.DS_Store
Thumbs.db

# 敏感信息
.env
*.key
```

- 已被 Git 追踪的文件不受 `.gitignore` 影响，需先 `git rm --cached <file>` 取消追踪
- `git check-ignore -v <file>` 检查文件被哪条规则忽略

## 其他命令

### patch
```shell
git show <commit_id> > patch.patch   # 生成补丁
git apply patch.patch                # 应用补丁
```

### clean
```shell
git clean -f      # 删除 untracked 文件
git clean -fd     # 删除 untracked 文件和目录
git clean -fx     # 包括 .gitignore 中的文件
git clean -n      # 预览要删除的文件（不实际删除）
```
