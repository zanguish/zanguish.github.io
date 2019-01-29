---
title: git 常用操作
date: 2018-12-18 22:39:50
tags:
  - git
---

#### 1. Git回滚代码到某个commit
```
回退命令：
$ git reset --hard HEAD^         回退到上个版本
$ git reset --hard HEAD~3        回退到前3次提交之前，以此类推，回退到n次提交之前
$ git reset --hard commit_id     退到/进到 指定commit的sha码

强推到远程：

$ git push origin HEAD --force
```


#### 2.Git 远程仓库 

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

#### git配置相关

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


#### gitignore不起作用解决办法
```
git rm -r --cached .
git add .
git commit -m 'update .gitignore'
```