# Git简介

Git是目前世界上最先进的**分布式版本控制系统**.

集中式版本控制系统, 版本库集中存放在中央服务器, 必须联网才能工作. (如CVS, SVN)

分布式版本控制系统没有中央服务器, 每个节点都是一个完整的版本库, 其安全性高, 不需联网也可使用.

##安装Git

Linux

```
$ sudo apt-get install git
```

##创建版本库(repository)

可以理解成一个目录, 里面所有文件可以被Git管理起来

把文件往Git版本库里添加或修改, 都是两步走: 先是git add, 然后git commit

```
$ mkdir git #创建一个空目录
$ cd git
$ pwd #用于显示当前目录
$ git init #初始化空目录
$ ls -ah #显示所有目录(包括隐藏文件)
.  ..  .git

******编写一个readme.md文件放在git目录下***
$ git add readme.md
$ git commit -m "wrote a readme file"
```



# 时光穿梭机

```
$ git status #查看当前仓库状态, 告诉你哪些文件被修改过
$ git diff readme.md #查看修改内容
```

### 版本回退

```
$ git log #查看历史记录
$ git log --pretty=oneline #查看历史记录, 一条条的显示
$ git reset -hard HEAD^ #回退到上一个版本, HEAD指向当前版本
$ git reset -hard 326382 #回退到任意一个版本, 数字是commit id, 在之前的git log里面可以看到
$ git reflog #查看命令历史, 因此可以去过去任意一个版本, 也可以去之前去过的未来
```

### 工作区和暂存区

Git和其他版本控制系统如SVN不同之处在于有**暂存区**的概念.

**工作区**(Working Directory): 即电脑里能看到的目录

**版本库**(Repository): 工作区有一个隐藏目录.git, 这个不算工作区, 是Git的版本库. Git的版本库里存了很多东西, 最重要的是**暂存区**(stage或是index), 还有Git自动创建的第一个分支**master**, 以及指向master的一个指针叫**HEAD**.

![git-repo](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384907702917346729e9afbf4127b6dfbae9207af016000/0)

我们创建Git版本库时, Git自动创建了唯一一个分支master, 所以git commit就是往master分支上提交更改.

可以理解为: 需要提交的文件修改通通放在暂存区, 然后一次性提交暂存区的所有修改.

所有可以一次性提交多个文件(比如readme.txt和LICENSE), 提交之后暂存区的状态就变成这样了:

![git-stage](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384907720458e56751df1c474485b697575073c40ae9000/0)

```
$ git diff           #是暂存区(stage)和工作区(work dict)的比较
$ git diff --cached  #是暂存区(stage)和分支(master)的比较
```

## 管理修改

Git为什么比其他版本控制系统设计的更加优秀, 在于Git跟踪并管理的是修改, 不是文件.

每次修改, 如果不add到暂存区, 那就不会加入到commit中.