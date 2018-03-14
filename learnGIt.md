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

**第1步**, 登录GitHub, 创建一个新的仓库, 名字叫Learn_PyTorch, 勾选**Initialize this repository with a README**, 这样会自动创建一个README.md文件

**第2步**, 从远程库克隆一个本地库:

```
$ git clone git@github.com:ANRhine/Learn_PyTorch.git #这个地址在GitHub上可以直接复制
```

GitHub给出的地址除了ssh之外, 还可以用https协议, 不过https速度慢, 每次推送必须输入口令.

# 分支管理

分支就是科幻电影里面的平行宇宙, 两个平行宇宙互不干扰, 不过在某个时间点, 两个平行宇宙可以合并.

假设一个项目, 大家协同合作, 你每天都有新的进度, 可以创建一个分支, 别人看不到, 你可以在自己的分支上干活, 想提交就提交, 直到开发完成, 再一次性合并到原来的分支上, 这样, 既安全又不影响别人工作.

## 创建与合并分支

在版本回退部分, 每次提交, Git都会把它们串成一条时间线(一串commit id), 这条时间线就是一个分支. 目前Git里只有一条主分支(master). HEAD严格来说不是指向提交, 而是指向master, master才是指向提交的, 所以HEAD指向的是当前分支.

1.一开始, master分支是一条线, Git用master指向最新的提交, 再用HEAD指向master, 就能确定当前分支, 以及当前分支的提交点, 每次提交，master分支都会向前移动一步，这样，随着你不断提交，master分支的线也越来越长：

![git-br-initial](https://cdn.liaoxuefeng.com/cdn/files/attachments/0013849087937492135fbf4bbd24dfcbc18349a8a59d36d000/0)

2.当我们创建新的分支，例如`dev`时，Git新建了一个指针叫`dev`，指向`master`相同的提交，再把`HEAD`指向`dev`，就表示当前分支在`dev`上：

![git-br-create](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384908811773187a597e2d844eefb11f5cf5d56135ca000/0)

3.从现在开始，对工作区的修改和提交就是针对`dev`分支了，比如新提交一次后，`dev`指针往前移动一步，而`master`指针不变：

![git-br-dev-fd](https://cdn.liaoxuefeng.com/cdn/files/attachments/0013849088235627813efe7649b4f008900e5365bb72323000/0)

4.假如我们在`dev`上的工作完成了，就可以把`dev`合并到`master`上。最简单的方法，就是直接把`master`指向`dev`的当前提交，就完成了合并：

![git-br-rm](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384908867187c83ca970bf0f46efa19badad99c40235000/0)

5.合并完分支后，甚至可以删除`dev`分支。删除`dev`分支就是把`dev`指针给删掉，删掉后，我们就剩下了一条`master`分支：

![git-br-rm](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384908867187c83ca970bf0f46efa19badad99c40235000/0)

```
$ git checkout -b dev #创建dev分支并切换, 
*****上面这条命令相当于下面两条命令
$ git brach dev #创建dev分支
$ git checkout dev #切换至dev分支
*********************
$ git branch #列出所有分支
*****在dev分支上修改提交后切换到master分支*****
$ git checkout master #切换至master分支
$ git merge dev #将dev分支的工作成果合并到master分支上
$ git branch -d dev #删除dev分支
$ git brach #发现只有master分支了
```

## 解决冲突

当Git无法自动合并分支时, 就必须首先解决冲突. 解决冲突后, 再提交, 合并完成.

用git log --graph命令可以看到分支合并图

## 分支管理策略

通常，合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

下面实战`--no-ff`方式的`git merge`：

```
$ git checkout -b dev #创建并切换到dev分支
$ git add README.md #在对dev分支修改后提交
$ git commit -m "add branch"
$ git checkout master #切换到master分支
$ git merge --no-ff -m "merge with no-ff" dev #准备合并dev分支, --no-ff参数表示禁用Fastforward
$ git log --graph --pretty=oneline #看一下分支历史
$ git branch -d dev #删除dev分支
```

**分支策略**

1.master分支应该是非常稳定的, 一般仅仅用来发布新版本, 平时不能在上面干活

2.dev分支是不稳定的, 到版本发布的时候, 再把dev分支合并到master上, 在master分支发布版本

3.团队协作, 每个人都在dev分支上干活, 都有自己的分支, 时不时地往dev分支上合并就可以了

4.Git分支非常强大, 在团队开发中应该充分应用

5.合并分支时, 加上--no--ff参数可以用普通模式合并, 合并后的历史有分支, 能看出来做过分支, 而fast forward合并看不出曾经做过合并

团队分支看起来像这样:

![git-br-policy](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384909239390d355eb07d9d64305b6322aaf4edac1e3000/0)

## Bug分支

临时接到bug修复任务时, 手头的工作还没有完成没法提交, Git提供了一个stash功能, 可以把当前工作现场"储藏起来", 等以后恢复现场后继续工作:

```
$ git stash #把当前工作现场“储藏”起来，等以后恢复现场后继续工作
$ git checkout master #假定需要在master分支上修复, 就从master创建临时分支
$ git checkout -b issue-101 #创建bug修复分支并切换
*****然后修复bug并且提交*****
$ git checkout master #切换回master
$ git merge --no-ff -m "merged bug fix 101" issue-101 #将issue-101分支合并
$ git branch -d issure-101 #删除issue-101分支
*****回到dev分支继续工作****
$ git stash list #查看刚才的工作现场
$ git stash pop #恢复工作现场并把stash内容删除
$ git stash list #查看发现没有任何stash内容

```

修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复bug，修复后，再`git stash pop`，回到工作现场。

## Feature分支

开发一个新feature，最好新建一个分支；

如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。

## 多人协作

```
$ git remote -v #查看远程库信息, 显示可以抓取和推送的origin地址
origin	git@github.com:ANRhine/git.git (fetch)
origin	git@github.com:ANRhine/git.git (push)
```

**推送分支:**

```
$ git push origin master #推送主分支
$ git push origin dev #推送其他分支
```

1.master分支是主分支, 因此要时刻与远程同步;

2.dev分支是开发分支, 团队所有成员都需要在上面工作, 所有也需要与远程同步;

3.bug分支只用于在本地修复bug, 没必要推到远程;

4.feature分支是否推到远程, 取决于你是否和你的小伙伴合作在上面开发.

**抓取分支:**

```
$ git clone git@github.com:ANRhine/git.git #模拟小伙伴从远程库clone
$ git checkout -b dev origin/dev #如果要在dev分支上开发, 则需要创建本地dev分支
*****如果两人对同样的文件作修改, 并试图推送, 会产生冲突*****
$ git pull #先用git pull把最新的提交从origin/dev抓下来, 然后, 在本地合并, 解决冲突, 再推送
$ git branch --set-upstream dev origin/dev #git pull之前要先指定本地dev分支与远程origin/dev分支的链接, 再pull
$ git pull
```

因此, 多人协作的工作模式通常是这样的:

1.首先，可以试图用`git push origin branch-name`推送自己的修改;

2.如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；

3.如果合并有冲突，则解决冲突，并在本地提交；

4.没有冲突或者解决掉冲突后，再用`git push origin branch-name`推送就能成功！

如果`git pull`提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream branch-name origin/branch-name`。

# 标签管理

发布一个版本时, 通常先在版本中打一个标签(tag), 这样, 就唯一确定打标签时刻的版本. 将来去论什么时候, 取某个标签的版本, 就是把那个打标签的时刻的历史版本取出来. 所以, 标签也是版本库的一个快照.

Git的标签虽然是版本库的快照, 但其实它就是指向某个commit的指针(和分支很像, 但分支可以移动, 标签不能移动), 所以, 创建和删除标签都是瞬间完成的.

## 创建标签

```
$ git branch master #切换到要打标签的分支上
$ git tag v1.0 #打标签
$ git tag #查看所有标签
$ git log --pretty=oneline --abbrev-commit #找到历史提价的commit id, 为之前提交的内容打标签
$ git tag v0.9 3293 #比如对应的commit id是3293, 打上标签
$ git tag #查看所有标签
$ git show v0.9 #查看标签信息
$ git tag -a v0.1 -m "verison 0.1 released" 439489 #创建带有说明的标签
$ git show v0.1 #查看标签信息
```

## 操作标签

```
$ git tag -d v0.1 #删除标签
$ git push origin v1.0 #将标签推送到远程
$ git push origin --tags #一次性推送所有标签
******标签已经推送到远程, 要删除怎么办*****
$ git tag -d v0.9 #先从本地删除
$ git push origin :refs/tags/v0.9 #再从远程删除

```

