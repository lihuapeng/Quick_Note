# Git笔记

## 版本库

版本库又名仓库，英文名repository，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。所有的版本控制系统，只能跟踪文本文件的改动。

- 基本操作

```shell
# 安装git后初始设置用户名和邮箱
git config --global user.name "Your Name"
git config --global user.email "email@example.com"

# 切换目录
cd <dir>

# 创建新目录
mkdir <dir>

# 仓库目录初始化
git init 

# 添加文件到仓库
git add <file> 

# 提交文件到仓库（可一次性提交多个add的文件）
git commit -m "add a file." 

# git 状态
git status 

# 变更比较
git diff <file>

# 日志查看
git log 

# 查看日志的commit id
git log --pretty=oneline

# 版本回溯（在Git中，用HEAD表示当前版本，也就是最新的提交的commit id，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100）
git reset --hard <commit id> 或 git reset --hard HEAD~1

# git 操作记录（提交变更和回溯的操作记录）
git reflog 

# 工作区删除修改
git restore <file>

# 暂存区删除修改
git restore --staged <file>

# 版本库删除
git rm <file>
```

## Git原理

Git跟踪并管理的是修改，而非文件。

工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。

```shell
Git区域：工作区(working)->暂存区（stage）->分支（master）

对应代码：代码编辑->git add->git commit
```

## 远程仓库

> 注意：请勿将公司内容连接公网git远程库（github，gitee）

Git是分布式版本管理系统，离线单机和联网均可。

```shell
# 创建SSH key
ssh-keygen -t rsa -C "youremail@example.com"

# 查看远程库信息
git remote -v

# 关联远程库
git remote add origin <git address>

# 删除远程库
git remote rm origin 

# 克隆远程库
git clone <git address>

# 本地库推送到远程库
git push -u origin master

# 拉取远程库
git pull origin master
```

## 分支

你创建了一个属于你自己的分支，别人看不到，还继续在原来的分支上正常工作，而你在自己的分支上干活，想提交就提交，直到开发完毕后，再一次性合并到原来的分支上，这样，既安全，又不影响别人工作。

```shell
# 查看分支
git branch

# 创建分支
git branch <branch name>

# 切换分支
git switch <branch name> 或 git checkout <branch name>

# 创建并切换分支
git switch -c <branch name> 或 git checkout -b <branch name>

# 合并指定分支到当前分支
git merge <branch name>

# 删除分支
git branch -d <branch name> 

# 分支合并路线图
git log --graph --pretty=oneline --abbrev-commit

# 禁用fast forward模式merge
git merge --no-ff -m "merge with no-ff" <branch name>

# 分支工作暂存切换主分支(解决BUG分支)
git stash 

# 工作暂存列表
git stash list

# 工作暂存恢复
git stash apply(stash内容不删除) 或 git stash pop(stash内容删除)

# 工作暂存删除
git stash drop 

# BUG分支合并工作stash
git cherry-pick <commit id>

# 丢弃未合并的分支
git branch -D <branch name>

# 变基
git rebase
```

> 分支冲突合并：把Git合并失败的文件手动编辑内容，再提交！

> 在分支下进行的工作，如果不commit的话，回到master，就会显示出在分支下添加的工作。这个时候，在master下修改完bug提交后，正在分支进行的工作也会提交了。为了避免这个情况，就在分支下git stash将工作隐藏，这个时候，切换到master时候，修改了bug，提交。分支的内容不会被提交上去。

### 分支开发指南

1. master分支是主分支，因此要时刻与远程同步；

2. dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；

3. bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；

4. feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

### 多人协作模式

1. 首先，可以试图用git push origin <branch-name>推送自己的修改；

2. 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；

3. 如果合并有冲突，则解决冲突，并在本地提交；

4. 没有冲突或者解决掉冲突后，再用git push origin <branch-name>推送就能成功！

5. 如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to <branch-name> origin/<branch-name>。

## 标签 

表明版本号

```shell
# 新建标签
git tag <tagname>

# 查看所有标签
git tag 

# 指定标签信息
git tag -a <tagname> -m "blablabla..." <commit id>

# 查看标签信息
git show <tagname> 

# 删除标签
git tag -d <tagname>

# 推送标签到远程
git push origin <tagname>

# 一次性推送全部本地标签
git push oririn --tags

# 删除远程标签
git push origin :refs/tags/<tagname>
```
