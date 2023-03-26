# Git 

## 定义

免费开源的分布式版本管理系统

## 操作

1.创建仓库

本地项目文件夹，输入 **$git init** ,创建本地仓库

2.克隆项目

在托管平台，如GitHub 获得‘url’， 通过 **git clone ‘url’**克隆该项目

3.文件的状态

①创建本地仓库后，仓库里的文件都处于未跟踪状态

②通过**git add <name>** 将文件状态变为暂存状态

③通过**git commit** 将文件提交进行版本的迭代，此时文件状态变为未修改状态且已跟踪状态

④此时可以通过使用**git rm <name>** 将已跟踪状态文件变为未跟踪状态文件

或者修改文件使文件处于已修改状态，并通过**git add <name>**将已修改状态文件变为暂存状态文件，使用**git commit**进行版本的新的迭代（此时文件状态已从暂存状态变为未修改状态）

![image-20230326211333238](C:\Users\undercurrent\AppData\Roaming\Typora\typora-user-images\image-20230326211333238.png)