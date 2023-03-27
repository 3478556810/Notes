# Git 

## 定义

免费开源的分布式版本管理系统

## 操作

### 1.创建仓库

本地项目文件夹，输入 **$git init** ,创建本地仓库

### 2.克隆项目

在托管平台，如GitHub 获得‘url’， 通过 **git clone ‘url’**克隆该项目

### 3.文件的状态、提交版本

使用git status查看文件的状态，红色的为已修改但未暂存，绿色为已暂存

其他指令：

git diff <name> ：查看文件修改的部分

git log --all：查看之前所有的提交历史

git log pretty：查看精修的提交历史，如 

```bash
git log --pretty=oneline
```

，将提交日志以一行展示

git log --graph：以图形化方式查看提交历史

#### 状态详解：

①创建本地仓库后，仓库里的文件都处于未跟踪状态

②通过**git add <name>** 将文件状态变为暂存状态

③通过**git commit** 将文件提交进行版本的迭代，此时文件状态由暂存状态变为未修改状态且已跟踪状态

（tips： **git commit -m ‘xxx’**，提交暂存的文件，git commit -am 'xxx'，提交没有暂存的文件，其中xxx为本次提交的描述内容）

通过**git reset head~ --soft** 来取消本次的提交（不能撤销第一次提交）

④此时可以通过使用**git rm <name>** 将已跟踪状态文件变为未跟踪状态文件

或者修改文件使文件处于已修改状态，并通过**git add <name>**将已修改状态文件变为暂存状态文件，使用**git commit**进行版本的新的迭代（此时文件状态已从暂存状态变为未修改状态）

![image-20230326211333238](C:\Users\undercurrent\AppData\Roaming\Typora\typora-user-images\image-20230326211333238.png)



### 4.远程仓库

1.链接远程仓库

git remote add <name> <remoteRepositoryUrl>

git remote：查看远程仓库

git remote rename <oldName> <newName>：更新远程仓库名称

git push <remoteRepositoryname> <branch>：将给定分支推送至远程仓库

（tips: GitHub 当前已禁止密码登录，需使用tokens登录或者SSH密钥登录）

### 5.分支

![image-20230327095625686](C:\Users\undercurrent\AppData\Roaming\Typora\typora-user-images\image-20230327095625686.png)

1.查看当前分支

git branch --list：列出所有分支 带星号的为当前分支

2.创建分支

git branch <branch>：创建一个分支

3.切换分支

git checkout <branch>：切换到给定分支

git checkout -b <branch>：创建并切换到新分支

4.合并分支

