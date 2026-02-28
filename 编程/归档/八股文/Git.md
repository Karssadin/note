---
tags:
  - 编程
  - 八股文
up: 
down: 
relation:
  - "[[Linux]]"
  - "[[Linux下使用VS Code进行C++开发]]"
---
- [[#基础配置|基础配置]]
- [[#HEAD 指针与相对引用|HEAD 指针与相对引用]]
- [[#commit|commit]]
- [[#branch|branch]]
- [[#checkout 和 switch|checkout 和 switch]]
- [[#查看状态与历史（status、log）|查看状态与历史（status、log）]]
- [[#add、reset、restore、stash|add、reset、restore、stash]]
- [[#远程操作（fetch、pull、push、clone）|远程操作（fetch、pull、push、clone）]]
	- [[#远程操作（fetch、pull、push、clone）#先有本地库|先有本地库]]
	- [[#远程操作（fetch、pull、push、clone）#先有远程库|先有远程库]]
	- [[#远程操作（fetch、pull、push、clone）#远程分支更新|远程分支更新]]
- [[#merge、rebase|merge、rebase]]
- [[#标签管理（tag）|标签管理（tag）]]
- [[#patch|patch]]
- [[#cherry-pick|cherry-pick]]
- [[#clean|clean]]
- [[#diff|diff]]

- GIT希望提交记录尽可能地轻量，因此在你每次进行提交时，它并不会盲目地复制整个目录。条件允许的情况下，它会将当前版本与仓库中的上一个版本进行对比，并把所有的差异打包到一起作为一个提交记录。
## 基础配置

- git config --global user.name "Your Name"
- git config --global user.email "youremail@example.com"
- git pull origin develop 从远程(origin) 的 develop 分支拉取代码

## HEAD 指针与相对引用
- **`HEAD`**：`HEAD` 是一个特殊的指针，指向当前正在操作的提交`（commit）`。通常情况下，`HEAD` 指向某个分支`（branch）`，该分支指向具体的提交快照。
- **相对引用**：可以使用 `HEAD` 指针进行相对引用，例如 `HEAD~`、`HEAD^` 等。
----
- **`Git` 采用快照存储模型，而不是传统的增量存储。提交时会记录当前工作目录的快照，并通过父指针形成提交链。**
- `HEAD` 指向当前分支最新的提交快照。通过 `HEAD~` 或 `HEAD^` 可以访问历史提交。`Git` 中的每个提交对象包含：
    - **树对象**（保存文件快照）。
    - **父指针**（指向上一个提交对象）。
    - **提交信息**。
----
- `HEAD~` 或 `HEAD^`：表示上一个提交节点（`HEAD~1` 等价于 `HEAD^`）。
- `HEAD~2` 或 `HEAD^^`：表示上上一个提交节点，以此类推。
- `HEAD^i`：`^i` 表示第 `i` 个父提交节点。例如 `HEAD^2` 表示第二个父节点。
	- 在进行`merge`操作的时候会出现一个分支有多个`parent`的操作，以当前分支为`parent1`，`merge`的分支为`parent2`
- `git log HEAD~3..HEAD`：查看最近三次提交之间的变更。
- `git reflog`查看`head`指针指向的历史变动
## commit
- 提交时会记录当前工作目录的快照，并生成新的提交对象：
    - **提交对象**包含：提交信息、作者、时间戳、父指针和文件快照。
    - **更新指针**：提交后，当前分支指针会指向新提交，`HEAD` 指针也随之更新。
- `--amend` 会修改最近一次提交，实际上是创建新的提交对象，并更新分支指针指向新提交。
----
- `git commit`：创建新的提交。
- `git commit -m "<message>"`：带提交信息创建提交。
- `git commit --amend`：修改最近一次提交。
- `git commit --squash <commit_id>`：压缩合并多个提交。
## branch
- 分支实际上是指向某个提交的可变指针。创建分支时，只是在对应`commit_id`基础上创建一个新的指针。
- **删除分支**时，只是移除了指针，并不会删除提交历史。
- **重命名分支**只会修改分支指针的名字，并不会影响提交记录。

- `git branch <branch_name> (commit_id) `：创建分支但不切换：
	- 如果不指定`commit_id`，就会以当前`head`指向的`id`为基准创建分支指针
- `git branch -d <branch_name>`：删除分支。
- `git branch -D <branch_name>`：强制删除分支。
- `git branch -m <new_branch_name>`：重命名当前分支。
## checkout 和 switch
- `checkout` 或 `switch` 命令会将工作目录中的文件替换为目标提交的快照，HEAD 指针指向新分支。
- `checkout -b` 和 `switch -c` 会同时创建新的分支指针，并将 HEAD 指针移动到新分支上。
---
- `git checkout <commit_id>`：检出到指定提交。
- `git checkout <branch_name>`：切换`HEAD`到指定分支。
- `git checkout -b <branch_name> <commit_id>：创建并切换到新分支。
	- 如果不指定commit_id，就会以当前head指向的id为基准创建分支指针
- `git switch <branch_name>`：切换到指定分支（推荐使用 `switch` 而非 `checkout`）。
- `git switch -c <branch_name> <commit_id>：创建并切换到新分支。
	- 如果不指定`commit_id`，就会以当前`head`指向的`id`为基准创建分支指针
- 可以使用`checkout`来进行文件的回退，可以指定文件进行回退
- `git checkout <commit_id> <file_path>`
## 查看状态与历史（status、log）
- `status` 会比较工作目录、暂存区和 HEAD 指针所指向的提交快照的不同。
- `log` 命令通过遍历提交链表来展示历史记录，每个提交对象会包含提交信息和父指针。
---
- `git status`：查看工作目录和暂存区状态。
- `git log`：查看提交历史。
- `git log --oneline`：简洁查看提交历史。
- `git log --pretty=oneline`
- `git log --graph`：图形化展示提交历史。
- `git diff`：查看工作目录与暂存区的差异。
- `git diff <commit_id1> <commit_id2>`：查看两个提交之间的差异。
## add、reset、restore、stash
- **暂存区**是 Git 中的一个特殊区域，用于保存即将提交的更改快照。
- `add` 命令会将文件快照从工作目录复制到暂存区。
- `reset` 会修改 HEAD 指针位置，`--hard` 会覆盖暂存区和工作目录，`--soft` 只修改 HEAD 指针。
---
- `git add <file>`：将文件添加到暂存区。
- `git reset <file>`：从暂存区移除文件，但保留工作目录的修改。
- `git reset --hard <commit_id>`：将 HEAD 和工作目录都重置到指定提交。
- `git restore <file>`：恢复工作目录中文件到暂存区版本。
- `git restore --staged <file>`：将暂存区中的文件恢复到 HEAD 指针版本。

```
git stash  保存当前工作区和暂存区的修改状态,切换到其他分支修复 bug 等工作,然后在回来继续工作
git stash list  查看保存现场的列表
git stash pop   恢复的同时把 stash 内容也删除
git stash apply  恢复现场，stash内容并不删除
git stash drop   删除 stash 内容
git stash apply stash@{0}  多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash
 通常在 dev 分支开发时,需要有紧急 bug 需要马上处理,保存现在修改的文件等,先修复 bug 后再回来继续工作的情况
 
git cherry-pick <commit> 复制一个特定的提交到当前分支(当前分支的内容需要先 commit,然后冲突的文件需要解决冲突,然后 commit)
```

## 远程操作（fetch、pull、push、clone）


- `git fetch`：从远程仓库获取最新更新，但不合并到本地分支。
- `git pull`：获取并合并远程更新。
- `git push`：将本地分支提交推送到远程仓库。
- `git clone <url>`：克隆远程仓库到本地。


- `fetch` 会下载远程仓库的更新到 `.git` 目录中的 `FETCH_HEAD`，但不会修改工作目录。
- `pull` 是 `fetch` 和 `merge` 的组合操作，获取远程更新并自动合并。
- `push` 会将本地分支的更新提交到远程仓库，远程分支指针更新。
- `clone` 会复制远程仓库的 `.git` 数据库，并在本地创建工作目录。
### 先有本地库

```Shell
# 后面的地址换成自己的 GitHub 仓库地址
git remote add origin git@github.com:taididibear/test.git

# 也可以使用GitHub提供的命令
git remote add origin https://github.com/taididibear/test.git
```

- 这里的origin是一个名字，也可以是其他名字，比如下面的同时使用Github和Gitee
- 第一次创建名字之后使用命令`git push -u origin master`第一次推送master分支的所有内容push到远程仓库
- 之后的推送就可以省略-u，直接使用git push 远程库名字 master来push了

```Shell
git remote       #查看远程库信息
git remote -v   # 查看远程库详细信息
git remote rm origin  #删除已关联的远程库 origin
git push -u origin master
#第一次推送使用-u，将本地master推送到远程的origin，还会将origin的master和本地的master
git push origin master      #推送本地 master 分支到远程库
git push origin dev         #推送本地 dev 分支到远程库
#  除了第一次推送,不需要添加 -u 参数
```

同时使用Github和Gitee

```Shell
# 一个本地库关联多个远程库,例如同时关联 GitHub 和 Gitee:
# 1. 先关联GitHub的远程库：(注意:远程库的名称叫 github，不叫 origin)
git remote add github git@github.com:taididibear/test.git
# 2. 再关联Gitee的远程库：(注意:远程库的名称叫 gitee，不叫 origin)
git remote add gitee git@gitee.com:taididibear/test.git
# 3. 推送到远程库
git push github master
git push gitee master
```

### 先有远程库

```Shell
git clone git@github.com:taididibear/test.git
# 上面使用的是ssh协议，可以改成https协议，但是ssh协议快

git clone https://github.com/taididibear/test.git
```


### 远程分支更新
- 取远程的分支更新，新增分支……
	- 如果clone的时候没有指定--single-branch，远程有了新分支，直接`git fetch`就可以使用checkout切换分支了
	- 如果指定了--single-branch，则需要重新add
		- `git remote set-branches --add origin [remote-branch]`
		- `git fetch origin [remote-branch]:[local-branch]`
		- `git branch --set-upstream-to=origin/remote-branch local-branch`

## merge、rebase
- `git merge <branch_name>`：合并指定分支到当前分支。
- `git merge --squash <branch_name>`：合并分支但不生成合并提交。
- `git rebase <branch_name>`：将当前分支变基到指定分支上。
- `git rebase -i <commit_id>`：交互式变基，合并、修改或删除提交。


- `merge` 会创建一个新的合并提交，包含两个父指针。
- `rebase` 会重新应用当前分支的提交到目标分支的最新提交，相当于“移动基线”。
- `rebase -i` 提供交互界面，可以对多个提交进行压缩、修改等操作，简化提交历史。


## 标签管理（tag）

- 标签是指向特定提交的引用，可以是轻量标签（仅包含名称）或注释标签（包含额外信息）。

- `git tag <tag_name>  # 创建标签 
- `git tag -a <tag_name> -m "message" ` # 创建带注释的标签
- ` git push origin <tag_name> ` # 推送标签到远程 
- `git tag -d <tag_name>`  # 删除本地标签 
- `git push origin :refs/tags/<tag_name>`  # 删除远程标签`


## patch
- `git show <commit_id> > patch.patch`
- `git apply patch.patch`

## cherry-pick
- `git cherry-pick <commit_id>`
	- 将另一个分支对应的commit pick到当前分支，与merge不同，merge会将当前head之后的分支都进行pick。这个是单独pick
## clean
- `git clean -[]`：默认-f
	- f：untracked文件
	- d：untracked目录
	- x：gitignore文件
	- n：查看当前命令要清理的文件列表
## diff

```Shell
git diff
git diff <file>
git diff --cached
git diff HEAD -- <file>
# git diff 查看工作区(work dict)和暂存区(stage)的区别
# git diff --cached 查看暂存区(stage)和repository的区别
# git diff HEAD -- <file> 查看工作区和repository里面最新版本的区别
# 如: git diff readme.txt  表示查看 readme.txt 修改了什么,有什么不同
```


