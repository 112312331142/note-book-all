# git教程

## git零基础快速掌握本地文件到远程仓库

````
git config --global user.name "名字"//不是韦一敏
git config --global user.email "邮箱地址"
git init
git add .
git commit -m '描述'
git remote add origin 仓库地址
git branch -M main
git push -u origin main
````





## 一、git简介

 git：分布式版本控制软件

git版本号：40位十六进制数字：9ab95564d450f0d28e649b00148c2c5287f076fa

文件创建时，提交会创建三个版本号：提交信息、文件状态、文件内容

找版本号之间的关系：` git cat-file -p  dfe0770424b2a19faf507a501ebfc23be8f54e7b`

将文件里的文件都定义为安全文件（解决用户不一致问题）：`git config --global --add safe.directory "*"`



## 二、git指令

`git -v`||`git --version`： 查看软件版本

` git config [--global] user.name itchen`||`git config [--global] user.email 821285060@qq.com`：给仓库配置名字和邮箱地址，不叫global表示配置单个文件

`git rm --cached a.txt`：将文件从暂存区变为为跟踪的状态

`git log [--oneline]` ：显示修改文件日志

`git restore filename`：恢复删除但未提交的软件

`git reset --hard 21b00dc`：版本回退到之前，‘“21b00dc”版本号的前六位数字，但会丢失版本

`git revert 52e1480`：撤销之前引入的修改，不会丢失版本，相当于提交一个撤销删除的提交

 `git tag tagName 版本号`：给标签添加版本号

` git remote add origin git@github.com:112312331142/remote-test.git`：添加远程url

## 13条常用git指令

### 1.git init

初始化一个新的Git仓库，这将在当前目录中创建一个名为“.git”的子目录，Git会将所有的元数据存储其中。

### 2.git clone

`git clone <仓库链接>`||`git clone <仓库链接> 别名`

克隆一个已存在的仓库。这会创建一个本地仓库的副本，包括其所有的历史记录和分支。链接是HTTPS形式的

### 3.git add

`git add file1 file2`或`git add .`

将修改内容添加到下一次提交当中。这将把指定的文件添加到暂存区中，这些文件将包含在下一次提交中。

### 4.git commit

`git commit -m "描述信息"`

创建一个新的提交。这将记录暂存区的修改以及自上次提交以来所做的任何其他修改，并附带一条描述这些修改的提交信息。

### 5.git push

`git push -u origin main`

将提交推送给远程仓库。这将把本地的的提交发送给指定的远程仓库，更新远程分支以包含新的提交。

### 6.git pull

`git pull origin main`

从远程仓库获取并合并修改。这会从指定的远程仓库中获取最新的提交，并将其合并到当前分支中。

### 7.git branch

`git branch new-branch`

列出、创建或删除分支。这个命令可以用来列出仓库中可用的分支，创建新的分支或删除现有的分支。

`git branch -d user`：删除user分支（不能删除自己所在的分支）

### 8.git checkout

`git checkout new-branch`

将一个分支合并到另一个分支。这个命令允许你切换到仓库中的不同分支，并将其作为当前工作分支。

`git checkout -b order`：创建并切换分支到order

### 9.git merge

`git merge other-branch`

将一个分支合并到另一个分支。这个命令将一个分支的修改合并到另一个分支中，创建一个反映合并变化的新提议。

### 10.git status

显示仓库的状态。这个命令会显示当前分支、任何暂存或未暂存的修改以及任何未跟踪的文件

on branch master：显示在哪个分支

Untracked files(未追踪状态)→git add  . →

Changes to be committed:(文件在暂存区中)→git commit →

nothing to commit, working tree clean:(工作区没有内容)

### 11.git rebase

将一个分支的修改合并到另一个分支。

假设你在“abc”分支上进行了一些修改，希望将这些修改合并到“main”分支中。可以使用git rebase命令将修改重新应用到main分支中。

### 12.git stash

临时保存还未准备提交的修改。

### 13.git revert

撤销之前提交引入的修改。



三、gitLab