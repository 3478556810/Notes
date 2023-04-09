# Git 

## 定义

免费开源的分布式版本管理系统

## 操作

### 1.创建仓库

本地项目文件夹，右键打开git bash,输入 

```bash
git init
```

 ,创建本地仓库

### 2.克隆项目

在托管平台，如GitHub 获得‘url’， 通过 

```bash
git clone ‘url’
```

克隆该项目

### 3.文件的状态、提交版本

使用

```bash
git status
```

查看文件的状态，红色的为已修改但未暂存，绿色为已暂存

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

②通过

```bash
git add <name>
```

 将文件状态变为暂存状态

③通过

```bash
git commit 
```

将文件提交进行版本的迭代，此时文件状态由暂存状态变为未修改状态且已跟踪状态

④此时可以通过使用

```bash
git rm <name>
```

 将已跟踪状态文件变为未跟踪状态文件

或者修改文件使文件处于已修改状态，并通过

```bash
git add <name>
```

将已修改状态文件变为暂存状态文件，使用

```bash
git commit
```

进行版本的新的迭代（此时文件状态已从暂存状态变为未修改状态）

![image-20230326211333238](C:\Users\undercurrent\AppData\Roaming\Typora\typora-user-images\image-20230326211333238.png)

git checkout -- <name>：使给定文件返回未修改状态,放弃之前的修改内容

（tips：简洁语法： git commit -m ‘xxx’，提交暂存的文件，git commit -am 'xxx'，提交没有暂存的文件，其中xxx为本次提交的描述内容）

通过git reset head~ --soft 来取消本次的提交（不能撤销第一次提交）

### 4.远程仓库

1.链接远程仓库

```bash
git remote add <name> <remoteRepositoryUrl>
```

git remote：查看远程仓库

git remote rename <oldName> <newName>：更新远程仓库名称

git push <remoteRepositoryname> <branch>：将给定分支推送至远程仓库

git push -u <remoteRepositoryname> <branch>：此次推送之后将默认推送给定远程仓库的分支，使用git push简化操作

（tips: GitHub 当前已禁止密码登录，需使用tokens登录或者SSH密钥登录）

### 5.分支

![image-20230327095625686](C:\Users\undercurrent\AppData\Roaming\Typora\typora-user-images\image-20230327095625686.png)

1.查看当前分支

```bash
git branch --list
```

列出所有分支 带星号的为当前分支

git branch <branch>：创建一个分支

git checkout <branch>：切换到给定分支

git checkout -b <branch>：创建并切换到新分支

4.合并分支

```bash
git merge <branch>
```

将给定分支合并到当前分支

5.拉取分支

```bash
git fetch
```

 将远程分支拉取到本地

6.新建跟踪分支

```bash
git checkout -b <branch>  <remoteRepositoryname> <branch>
```

或者

```bash
git checkout --track  <remoteRepositoryname> <branch>
```



### 6.贮藏

1.贮藏当前修改的内容

```bash
git stash（push）
```

2.恢复贮藏的内容

git stash apply

或者

```bash
git stash apply stash@{no.} 
```

恢复指定序号的stash内容

git stash pop：恢复最后一次贮藏的内容，恢复完成后直接删掉该stash

3.查看贮藏的内容

```bash
git stash list
```

4.删除指定的贮藏内容

```bash
git stash drop stash@{no.}
```

删除指定序号的stash内容

stash@{0}:

序号0为最新内容，随着序号的增大内容提交时间越早

### 7.重置、变基

1.重置

```bash
git reset head~ --soft
```

撤销本次提交

其中，head后的‘~’表示回退一个快照，head~2表示回退二个快照，以此类推。

--soft：表示只是撤销此次的提交，通过git add设置的暂存状态仍是存在的，不加则没有暂存状态

--hard：不仅取消文件的暂存状态，还把之前的修改内容也取消，回到上一次提交的状态

2.变基

```bash
git rebase -i head~3
```

：对前三次提交的一个分支的进行变基
