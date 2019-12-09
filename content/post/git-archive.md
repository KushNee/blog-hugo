+++
title = "git 常用命令整理"
lastmod = 2019-01-18T13:07:35+08:00
tags = ["工具", "整理"]
draft = false
author = "Kush Nee"
+++

- 重命名  `git mv <source> <target>`
- 查看日志

  - `git log`
  - 简洁模式 `--oneline`
  - 指定查看个数（从新到旧）`-n<number>`
  - 查看全部分支日志`--all`
  - 图形化`-—graph`
<!--more-->

- 修改 commit 的 message

  - 修改最近一次`git commit —amend`

  - 修改之前的

    - `git rebase -i <parent-commit-hash>`
    - `reowrd/r <rename-commit-hash> <original-commit-meesage>`
    - 新界面与提交 message 编辑界面相同，直接修改保存

- 合并 commit
  - `git rebase -i <parent-commit-hash>`
  - 合并连续 commit
    - `pick`指定需合并 commit 中最旧的一个作为合并用
    - `squash/s`指定被合并的 commit
  - 合并不连续 commit
    -  `git rebase -i <parent-commit-hash>`
    - 需要合并的 commit 修改到一起
    - `pick/p`指定需合并 commit 中最旧的一个作为合并用
    - `squash/s`指定被合并的 commit
  - 添加 message
- 比较
  - 比较暂存区和 head `git diff --cached`
  - 比较工作区和暂存区`git diff`
- 恢复
  - 全部恢复暂存区至与 head 一致`git reset HEAD`
  - 工作区恢复至暂存区`git checkout -- <file>...`
  - 部分恢复暂存区至与 head 一致`git reset HEAD -- <file>...`
  - 回退连续 commit `git reset --hard <commit-hash>`
- 比较不同分支/提交的差异
  - `git diff <branch>/<commit-hash> <branch>/<commit-hash>`
  - 比较指定文件`git diff <branch>/<commit-hash> <branch>/<commit-hash> -- <file>`
- 删除`git rm <file>`
- 临时保存工作区`git stash`
  - 显示 stash `git stash list`
  - 恢复`git stash apply`/`git stash pop`
- 添加远端仓库`git remote add <remote-name> <remote-url>`
- push 时同时为新创建的 branch 设置上游`git push --set-upstream <remote-name> <branch-name>`
