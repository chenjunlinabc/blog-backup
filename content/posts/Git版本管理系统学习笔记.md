---
title: "Git版本管理系统学习笔记"
categories: [ "学习" ]
tags: [ "Git" ]
draft: false
slug: "30"
date: "2021-06-16 14:51:00"
---

git安装

Windows和mac

到git官网下载对应的安装文件，进行安装，下一步

Linux

sudo apt install git-all
或者
sudo dnf install git-all

也可以用源代码编译（Git是开源的，是Linux之父的作品）
https://github.com/git/git/releases



方便git管理，记录每一个修改了Git仓库的人，设置用户名和邮箱

git config --global user.name "chenjunlin"
git config --global user.email "a@xiaochenabc123.test.com"


ssh-keygen -t rsa -C "a@xiaochenabc123.test.com" # 生成ssh密钥


mkdir learngit #创建一个空目录

cd learngit # cd到目录

pwd #显示当前目录路径

git init #将当前的目录变成git管理仓库

ls -ah #将所有目录（包括隐藏目录）显示出来

git add 文件名 #将文件添加到仓库中

git commit -m 说明 #告诉git这次添加到仓库的说明，可以是任意内容

git status #返回仓库的当前状态

git diff 文件名 #查看当前文件的内容

git log #查看历史记录，加上--pretty=oneline 查看commit id（版本号）

git reset --hard HEAD^ #回退上一个版本，上上一个版本就是HEAD^^，以此类推，或者使用HEAD~ 就是要回退多少个版本

git reset --hard commit id #回到“未来”的版本，版本号可以不用写全，git会自动搜索，过版本号长度要尽量的长，防止可能出现搜索到多个版本的情况

git reflog #查看命令历史，也被用于找回commit id

git reflog show和git log -g --abbrev-commit --pretty=oneline效果是一样的

git add 实际上是把要提交文件存在暂存区，git commi 是一次性把暂存区的文件提交到分支，每一次git commi都要小心！！！

git checkout -- 文件名 #把该文件从工作区的修改全部撤销掉（修改后还没有git add放在暂存区或者已经提交到暂存区后又修改了使用这个就回到提交到暂存区后的状态）

git reset HEAD 文件名 #将暂存区的修改撤销掉，重新放回工作区

git rm 文件名 #把该文件从版本库中删除

ssh-keygen -t rsa -C "邮箱" #创建SSH Key,在用户主目录，查看.ssh目录，再查看该目录 有没有id_rsa和id_rsa.pub文件，没有就执行该命令，id_rsa是私钥，id_rsa.pub是公钥，在github的Key文本框添加上id_rsa.pub文字的内容

git remote add origin git@github.com: github的账户名/仓库名.git #关联github仓库

git push -u 远程仓库名 本地分支名 # 把本地分支推送到远程库上 -u是第一次添加要使用，本地的分支和远程的分支关联起来，关联了后，第2次推送就不用加了

git remote add 远程库名 远程库的git链接#本地和远程库关联起来

git clone git@git库地址 #克隆远程库到本地

git checkout -b 分支名 #创建dev分支并切换到该分支，也可以使用下面方法

git branch 分支名 #创建分支

git checkout 分支名 #切换到该分支

git branch #查看当前分支，会输出全部分支，在当前分支前加*

git checkout 分支名 #切换回指定分支

git merge 分支名 #可以把指定分支合并到当前分支

git branch -d 分支名 #删除该分支

git status #查看冲突的文件

<<<<<<<和=======和>>>>>>>表示不同分支的内容，修改冲突就是把不同分支提交的内容修改为一样的

一般都是先克隆主分支上的最新文件，然后在最新文件上进行操作，然后提交，避免分支冲突

git log #查看分支的合并图，可以在后面加上--graph

合并分支时，git可能会使用fast forward模式，在这种模式下删除分支会丢掉分支信息，在合并时候使用git merge --no-ff 分支名 表示禁用fast forward，可以加上-m参数，把commit说明加上，使用fast forward模式是看不出来做出过合并的

git stash #把还没有提交的工作现场存起来，可以重新恢复工作现场

git stash list #查看已经存起来的工作现场

git stash apply #恢复工作现场，但是不会删除stash的内容，需要使用git stash drop来删除

git stash pop #恢复工作现场并删除stash的内容

git stash apply stash@{指定} #可以指定恢复的stash

git branch -D 分支名#强制删除分支，在没有被合并的情况时

git remote #查看关联的远程库信息，或者加-v 查看更详细的信息，没有推送权限就没有push地址

git push 远程库名 分支名 #把该分支推送到远程库对应的分支上

git checkout -b 分支名 远程库名/分支名 #默认只有主分支，要使用其他分支上工作就使用这个

git branch --set-upstream-to=远程库名/分支名 分支名 #指定本地分支与远程分支的链接

git pull #把最新的提交抓下来

git rebase #把本地未push的分叉提交历史衍合成基线，使用该命令会导致本地的分叉提交被修改

git tag 标签名 #给对应的分支打上标签，git tag查看所有标签

git tag -a 标签名 -m "说明" 版本号

git tag 标签名 版本号 #标签链接版本号

git show 标签名 #查看标签信息

git tag -d 标签名 #删除指定的标签

git push 远程库名 标签名 #推送该标签到远程库上，或者--tags 一次性推送全部没有推送到远程库的本地标签

删除远程库的标签先删除本地标签，然后使用 git push 远程库名：refs/tags/标签名

git remote rm 远程库名 #删除已有的远程库

git stash # 暂存修改，将改变暂时存起来

git stash list #查看在 stash 中的缓存
然后恢复工作现场

合并冲突

合并冲突是因为不同的分支，修改了同一份文件，而git不知道以谁为准，因此提示合并冲突，只需要修改这个冲突文件，修改为某个分支的修改，执行git status查看冲突的文件，再合并就好了


git status #查看冲突的文件

git log #查看分支的合并图

git log --graph --pretty=oneline --abbrev-commit #简化信息

修改冲突的文件的内容，改成一样的


git merge --abort # 退出合并，当合并冲突发生时，但是有不想修改冲突，而是想取消合并时，指向该命令，git会尝试恢复到执行合并前的状态


版本冲突

版本冲突就是当前版本不是最新的，git不知道以哪个版本为准，因此提示版本冲突，只需要git pull获取最新版本，就可以避免这个问题，如果已经发生版本冲突了，那么执行git status查看冲突文件，不管是分支冲突还是版本冲突，其修改都不会丢失，而是保存在这个冲突文件中，只需要手动修改成自己想要的就好了，一般来说上面是本地的，下面是最新的





---




版本控制

获取git仓库

将本地目录转换为git仓库（初始化仓库）

克隆已存在的git仓库到本地


git的三种状态

已修改，已暂存，已提交



git正常的工作流程：

工作区修改文件 -> 要提交的文件暂存 -> 提交更新（永久性存储到git仓库）

工作区 -> 暂存区 -> git仓库

初始化仓库

git init  // 初始化当前目录为git仓库，初始化会创建.git的隐藏目录，.git目录包含初始的必要文件





工作区中的文件的4种状态


未追踪（untracked）：没有被git管理的文件，git之前的提交中没有该文件

未修改（unmodified）：工作区内容和git仓库中文件的内容保持一致

已修改（modified）：工作区内容和git仓库中文件的内容不一致

已暂存（staged）：工作区已被修改文件被放到暂存区，准备提交到git仓库中


检查文件的状态

git status

以精简的方式显示文件状态

未跟踪文件前面会有??标记

已跟踪文件前面会有A标记

git status -s

git status --short



跟踪新文件或者将已跟踪，已修改的文件存放在暂存区，将有冲突的文件标记为已解决冲突

git add 文件名或者目录名

暂存多个文件

git add .


提交更新（将暂存区提交到git仓库）

git commit -m 本次提交的描述


撤销对文件的修改（恢复到git仓库中的版本）

git checkout -- 要撤销的文件名


取消已暂存的文件（移除在暂存区的指定文件）

git reset HEAD 要移除的文件名


跳过暂存区

工作区 -> git仓库

会将全部已经被跟踪的文件暂存起来一并提交

git commit -a -m 本次提交的描述



移除文件

git仓库和工作区移除文件

git rm -r 要移除的文件名

git仓库移除文件，工作区保留

git rm --cached 要移除的文件名



忽略文件

将一些文件不纳入git管理

.gitignore配置文件

以\*开头的表示忽略，例如：\*.java，表示忽略所有.java文件

以#开头是注释

以/结尾的是目录，忽略任何目录下的指定的文件夹，例如：hallo/

以/开头的是防止递归，只忽略当前目录的指定文件，例如：/hallo.java

以!开头的是表示取反，哪怕忽略了java，就是匹配所有指定的文件，例如：!hallo.java


使用glob模式进行匹配（glob是简化的正则表达式）


glob模式


*匹配0个或者多个

[abc]匹配任何一个在括号里的字符

?只匹配一个任意字符

[0-9]：匹配所有0到9的数字

a/**/c：可以匹配任意中间目录，例如a/b/c


例如：



hallo/*.java  // 忽略hallo目录下的全部java文件


hallo/**/*.java // 忽略hallo目录以及全部子目录下的全部java文件




查看提交历史

按时间来排序全部提交历史，最近的提交在上面

git log

只查看最新的指定条数的提交历史

git log 6

在一行上查看指定条数的提交历史

git log 6 --pretty=oneline


在一行上查看指定条数的提交历史，并且按照输出格式来输出

git log --pretty=oneline: "%h | %an | %ar | %s"


%h为提交的简写哈希值

%an为作者名字

%ar为作者修订日期

%s为提交说明



回退指定版本

git reset --hard 版本id

旧版本返回新版本（获取新版本的id）

git reflog --pretty=oneline



---


github


远程仓库访问：https和ssh


https：必须要输入账号和密码

ssh：需要配置，配置成功，不需要输入账号和密码


将本地仓库上传到远程仓库


本地有仓库：


本地仓库和远程仓库进行关联
git remote add origin 远程仓库地址


推送本地仓库内容
git push -u origin master


如果本地没有仓库，先建立仓库，再关联，推送

git init -> git add -> git commit


如果已经关联过，推送过，可以直接git push推送



ssh key


生成ssh key

ssh-keygen -t rsa -b 4096 -C "a@xiaochenabc123.test.com"

如果是windows，会在C:\Users\用户名文件夹\.ssh下

id_rsa：私钥

id_rsa.pub：公钥

私钥可以通过计算推出公钥，公钥推不出私钥，因此密钥安全性很高，通过配置公钥到github中，本地的私钥可以确保，是本人在提交推送


检测是否配置ssh key成功

ssh -T git@github.com


https远程仓库

https://github.com/xxx/xxx.git

ssh远程仓库

git@github.com:xxx/xxx.git

---



简单来说就是公钥设置在github


git clone 远程库

再修改文件

git add .  // 提交文件到暂存区

git commit -m "xxx"  // 提交更新

git push -u -f origin master  // 推送到远程仓库，-f是强制推送（团队仓库别使用-f，个人无所谓）

注意：git对文件名大小写默认不敏感，因此修改hallo.py和Hallo.py是不认为被修改，在提交时没有记录，如果想开启大小写敏感可以找到项目的.git文件夹（隐藏文件，需显示）,找到config文件，修改ignorecase为false，或者直接用git config core.ignorecase false命令一键修改


---


git回滚

git reset HEAD^ // 回到add暂存区时（未commit）

git reset --hard HEAD^  // 回滚到上个版本，HEAD是当前版本的指向

git reset --hard commitId  // 回到指定版本，commitId使用git log查看（commit那一行的id）

注意：reset回退会删除之前版本的（例如原版本在第3次修改，回到第2版本，会将第3版本的修改删除（这个删除不是指实质删除，而是工作区的代码回到之前的版本了，在第2版本上，没第3版本了））

如果这是某个修改有错误，要回退时，但是又想保留之前版本的工作时就可以使用git revert -n commitId

这种需求常见于多人协同开发，当别人已经push提交，如果再使用reset回退的话，就会影响到别人的提交，因为版本冲突了，revert实质上就是恢复到某个版本，但是之前的版本不会被删除，而且是在当前最新版本的基础上创建了一个新的版本，当然在git上没有是绝对的删除，只要有commitId在就可以恢复


---



在git2.23版本中添加了git switch命令，该命令专门用来切换分支，功能和git checkout一致

git switch 分支名 // 切换到指定分支

git switch -c 分支名 // 创建该分支并且切换到该分支，和git checkout -b功能一致


---



解决分支无法合并

导致原因：不同的分支都有自己的新提交，git不知道以谁的为准

解决方法：手动解决，通过git status命令查看冲突的文件


---


git commit --amend // 对最近提交的message进行修改

git rebase -i commit的id // 修改指定提交的message，这里应该使用reword或者r，退出该文件后，git会弹出修改commit描述的文件

git rebase -i中有6种命令，分别是pick，reword，edit，squash，fixup，exec

pick：提交，可通过排序来变更提交的顺序，如果不想要该次直接删除整行

reword：和pick类似，但是它可以给你修改commit的message的机会

edit：提供机会来修改提交

squash：将多个或者两个提交合并成一个提交，压缩到其上方的提交，并且给机会来编写这次提交的message

fixup：和squash类似，不过合并的过程中，被合并的提交的message将丢失，压缩到其上方的提交，但是只保留较早的提交的message

exec：对提交执行shell命令

---



解决分支冲突的其他解决方案（本地分支与远程仓库分支合并，本地分支作为指定远程仓库分支的未来分支提交）


将远程分支拉取至本地
git fetch 远程仓库地址 分支名

查看远程分支
git branch -r

查看全部分支
git branch -a

将远程仓库分支与本地当前分支合并
git merge 远程仓库地址/分支名

将远程仓库分支的修改与本地当前分支合并（rebase可以将其他分支的提交作为当前分支的时间线的开头，也就是当前分支作为新的未来提交分支了）
git rebase 远程仓库地址/分支名



---




GPG签名（通过签名来确定提交者身份，git默认的用户及邮箱可以伪造，不安全）


查看git是否包含GPG命令（windows要在Git Bash 运行）

gpg --version

创建GPG keys

gpg --full-generate-key

第一个是选择加密算法，选择1就好，然后就是选择key的长度（选择4096），再然后就是选择密令的有效期（根据自己选择来，不想过期就选择0），然后名称，邮箱，最后选择o设置私钥密码就创建完成

查看生成的公钥

gpg --list-keys

查看私钥

gpg --list-secret-keys


关联GPG公钥到Github

gpg --armor --export 密钥ID


对commit签名

git config --global user.signingkey 密钥ID

开启git自动签名（也可以在git commit的时候使用-s参数）

git config --global commit.gpgsign true

查看git仓库签名信息

git log --show-signature


解决github网页签名问题

导入github签名

curl https://github.com/web-flow.gpg | gpg --import

信任签名并且用自己的密钥进行签名验证

gpg --sign-key github的key











