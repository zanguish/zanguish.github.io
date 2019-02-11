---
title: git 常用操作
date: 2018-12-18 22:39:50
tags:
  - git
---

#### Git 撤销操作
```
回退命令：
$ git reset --hard HEAD^         回退到上个版本
$ git reset --hard HEAD~3        回退到前3次提交之前，以此类推，回退到n次提交之前
$ git reset --hard commit_id     退到/进到 指定commit的sha码

强推到远程：

$ git push origin HEAD --force


git reset HEAD file 已暂存 撤销
git checkout -- file。 未暂存 撤销

-----------

已修改,未暂存
git checkout .
或者
git reset --hard 慎用


已暂存，未提交 撤销

git reset
git checkout .
或者
git reset --hard 慎用

已提交，未推送 撤销
git reset --hard origin/master

已推送 撤销
git reset --hard HEAD^
git push -f


git log --pretty=oneline
```


#### Git 远程仓库 

```
1.查看现有远程仓库的地址url
git remote -v 


2.设置远程仓库地址

//直接设置
git remote set-url origin <URL> 

//先删后添加
git remote rm origin 
git remote add origin <URL>

//编辑配置文件
.git/conf


3.添加多个远程仓库
git remote add origin <url1> 
git remote set-url --add origin <url2> 
git remote set-url --add origin <url3> 

```

#### Git 配置相关

```
查看系统
git config --system --list

查看全局
git config --global  --list

user.email=zw13476587420@163.com
user.name=zanguish
core.autocrlf=true
ssh.variant=ssh

当前仓库
git config --local  --list

设置
git config --global user.name "myname"
git config --global user.email  "test@gmail.com"

```


#### Git gitignore不起作用解决办法

```
git rm -r --cached .
git add .
git commit -m 'update .gitignore'
```


#### Git stash

```
如果希望保留本地的改动,仅仅并入新配置项, 处理方法如下:
git stash
git pull
git stash pop

如果希望用代码库中的文件完全覆盖本地工作版本. 方法如下:
git reset --hard
git pull



stash 用法
git stash save "message..." 保存当前进度
git stash list 暂存区列表
git stash drop  [<stash>] 删除进度,默认最新的进度
git stash clear 删除所有存储的进度。
git stash apply [--index] [<stash>] 不删除进度,恢复进度
git stash pop [--index] [<stash>] 恢复进度,删除进度
git stash branch <branchname> <stash> 基于进度创建分支
git stash show [<stash>] 展示进度

[--index] 参数：不仅恢复工作区，还恢复暂存区
[<stash>] 进度编号
```