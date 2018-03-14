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

## 撤销修改

```
$ git checkout -- readme.md #当改乱了工作区某个文件的内容,想要丢弃工作区的修改时,用这个命令
$ git reset HEAD readme.md #已经把内容git add之后想要撤回,需要先用这个命令,再用git checkout命令
$ git git reset -hard 326382 #已经把内容git commit之后想要撤回,就用版本回退命令
```

**注意**: 忘了用哪个命令的时候, 随时用**git status**, 会有详细提示噢~

## 删除文件

```
$ git rm test.md #从版本库中删除文件
$ git commit -m "remove test.md" #删除后也需要提交
$ git checkout -- test.md #误删只有可以恢复
```

#远程仓库

Git的杀手级功能之一: 远程仓库

注册GitHub账号, 免费获得Git远程仓库. GitHub支持SSH协议, 只要知道了你的公钥, 就可以确认你在推送.

**第1步**,创建SSH Key, 一路回车,使用默认

```
$ ssh-keygen -t rsa -C "youremail@example.com" 
$ ls -ah #会出现.ssh文件夹,内有id_rsa(私钥)和id_rsa.pub(公钥)两个文件
```

**第2步**,登录GitHub,打开"Account setting", "SSH Keys"页面:

然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容

点“Add Key”，你就应该看到已经添加的Key：

## 添加远程库

实现本地库与远程库同步, GitHub上的仓库既可以作为备份, 又可以让其他人通过该仓库来协作.

**第1步,** 登录GitHub, "Create a new repository", 创建一个新仓库, 保持默认设置创建即可.

**第2步**, 根据GitHub提示, 在本地的git仓库下运行命令:

```
$ git remote add origin git@github.com:ANRhine/git.git #ANRhine是自己的GitHub账户名, origin是Git远程库的默认叫法
$ git push -u origin master #把本地库的所有内容推送到远程库上, 第一次推送需要加上-u参数, 之后可简化
```

推送成功后, 可以立刻在GitHub页面中看到远程库的内容已经和本地一模一样.

从现在起, 只要本地做了提交, 就可以通过命令:

```
$ git push origin master #把本地master分支的最新修改推送至GitHub
```

现在, 你就拥有了真正的分布式版本库!

**SSH警告**

(这个虽然还没遇到, 也写上, 以防万一.)

当你第一次使用git clone或者git push时, 会得到一个警告, 需要你确认GitHub的Key的指纹信息是否真的来自GitHub的服务器, 输入yes回车即可.

## 从远程库克隆

上面是将先有本地库, 再有远程库的时候, 如何关联远程库.

现在, 让我们先创建远程库, 再从远程库克隆.

**第1步**, 登录GitHub, 创建一个新的仓库, 名字叫Learn_PyTorch, 勾选Initialize this repository with a README, 这样会自动创建一个README.md文件

**第2步**, 从远程库克隆一个本地库:

```
$ git clone git@github.com:ANRhine/Learn_PyTorch.git #这个地址在GitHub上可以直接复制
```

GitHub给出的地址除了ssh之外, 还可以用https协议, 不过https速度慢, 每次推送必须输入口令.