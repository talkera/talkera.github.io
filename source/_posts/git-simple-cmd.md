---
title: Git简单命令手册
date: 2018-02-01
categories: 其他
tags: [Git]
---
## 基本操作
```bash
#创建无代码仓库
git init --bare /path/to/repos.git

#设置提交人email和name
git config --global user.email "xxx"
git config --global user.name "xxx"

#下载代码
git clone user@host:/path/to/repos.git
# svn checkout

#切换分支
git checkout develop
# svn switch 'https://code.com/project/branches/dev'

#创建并切换分支
git checkout -b dev2 origin/dev2
# 将会在本地创建新分支dev2，并关联到远程仓库origin的dev2上，然后自动切到dev2上。

#提交修改：分两步
git add filename
git add -A
git add .
git commit
# git commit -a相当于add+commit 相当于 svn commit，但只是提交到本地仓库

```
## 与远程仓库的交互
```bash
#远程仓库管理
git remote -v #查看
git remote rm origin #删除与远程仓库关联
git remote add origin git@127.0.0.1:/data0/gitRepos/anxin.git
#添加与远程仓库关联
# origin user@host:/path/to/repos.git

#提交到远程仓库，相当于svn commit到远程仓库
git push origin dev:dev
git push origin master:master

#从远程仓库更新本地，相当于svn update
git pull --rebase origin dev:dev
git pull --rebase
```

### rebase(变基)
- `git pull` 相当于`git fetch` + `git merge`
- `git pull --rebase` 相当于`git getch` + `git rebase`

使用rebase可以自由控制合并的进度。

rebase的过程如果遇到冲突，会自动停止。通过编辑代码解决冲突的文件。修改完成后通过`git add`添加到stage
```
git add filename.php
```
然后执行continue命令继续rebase
```
git rebase --continue
```
如果实在无法解决冲突，使用abort回退到rebase开始前的状态
```
git rebase --abort
```
### 为git pull默认追加rebase项
```
git config branch.autosetuprebase always #仅修改本仓库设置
git config --global branch.autosetuprebase always #修改全局设置
```

### 为仓库指定上游仓库
`git push`的时候可带`-u`参数。如果当前分支与多个主机存在追踪关系，那么这个时候`-u`选项会指定一个默认主机
```bash
git push -u origin dev:dev
#-u相当于执行以下命令
git branch --set-upstream-to=origin/dev dev
```
`-u`或者 `--set-upstream-to`， 每个分支仅需要执行一次。
这样后面就可以不加任何参数使用`git push`和`git pull`

## 撤销和回滚
```bash
#未执行git add时取消文件的修改
git checkout -- filename.php
#相当于svn revert

#已执行git add，未commit的文件，取消add
git reset filename.php
#类似svn revert

#已执行git add 需要取消文件的修改
git reset filename.php
git checkout filename.php

#将文件还原到某版本
git log filename.php
git checkout <commitID> filename.php

#已commit的文件，要撤销上次提交
git reset HEAD~1
git reset <commitID> 撤销到某次提交之后的提交。但本地文件仍保持不变
git reset --hard HEAD~1
#撤销到某次提交之后的提交
#并且！本地源码也被还原！不可恢复！

#checkout和reset的区别：
#   checkout修改的是文件内容
#   reset撤销的是提交历史
#已经push到远程仓库的commit不允许reset！！

#git revert 生成一个新的提交类撤销某次提交，只能整个版本revert，不支持单个文件
git revert <commitID>
```

### 修改上次提交
```
#修改最后一次提交的注释，如果push则无法修改
git commit --amend

#修改最后一次提交者:修改user和email后amend
git config --global user.name='ziliang'
git commit --amend --reset-author

#修改更早的提交
git rebase -i HEAD~3 #当前版本的导数第三次
#再使用git commit --amend
```

## 日志
```
#查看某条提交更新内容
git show <commitID>

#查看最近2次更新内容
git log -p -2

#查看某文件更新内容
git log -p filename.php
```


### 设置别名日志美化
```
#每次展示10条
git config --global alias.ll "log -10 --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

git ll

#每次展示所有日志
git config --global alias.la "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

git la
```
