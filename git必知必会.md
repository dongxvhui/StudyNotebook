# git必知必会

## git安装（略）

## git 配置

配置用户名和电子邮箱地址

~~~
$ git config --global user.name "donghuihui"
$ git config --global user.email "1060487361@qq.com"
~~~

查看已有配置项

~~~
$ git config --list
~~~

查看某个环境变量设定（如用户名，邮箱等）

~~~
$ git config user.name
$ git config user.email
~~~

## 创建仓库

创建空目录

~~~
$ mkdir learngit
~~~

进入目录

~~~
$ cd learngit
~~~

显示当前目录

~~~
pwd
~~~

初始化仓库，将当前目录作为git仓库

~~~
$ git init
Initialized empty Git repository in D:/project/gitProject/learngit/.git/
~~~

将文件添加到暂存区

~~~
$ git add readme.txt
~~~

将暂存区内容添加到仓库中

~~~
$ git commit -m "wrote a readme file"
~~~

查看仓库当前的状态，显示有变更的文件

~~~
$ git status
~~~

比较文件的不同，即暂存区和工作区的差异

~~~
$ git diff readme.txt
~~~

版本回退

查看日志

~~~
$ git log

单行输出
$ git log --pretty=oneline
~~~

回退版本

~~~
$ git reset --hard HEAD^
（HEAD^:上个版本，HEAD^^:上上个版本，以此类推。HEAD~100:回退到前100个版本）
$ git reset --hard 80527（版本号Id,输入前几位即可）
~~~

猫一眼

~~~
$ cat readme.txt
~~~

查看历史操作命令

~~~
$ git reflog
~~~

从工作区向暂存区添加文件（git add)

![75955851001384907720458e56751df1c474485b697575073c40ae9000.png](D:\project\工作空间\mdfile\git图源\75955851001384907720458e56751df1c474485b697575073c40ae9000.png.jpg)

把暂存区的文件提交到当前分支

![759558510013849077337835a877df2d26742b88dd7f56a6ace3ecf000.png](D:\project\工作空间\mdfile\git图源\759558510013849077337835a877df2d26742b88dd7f56a6ace3ecf000.png.jpg)

注：Git管理的是修改，而不是文件，每次修改，必须先git add提交到暂存区，然后才能git commit提交到分支，没有add,则不会commit到分支。

撤销修改

~~~
工作区
$ git checkout -- readme.txt
暂存区（unstage ,把暂存区的修改放回到工作区）
$ git reset HEAD readme.txt
注：HEAD表示最新的版本
~~~

删除文件

~~~
本地删除
$ rm test.txt
从版本库中删除
$ git rm test.txt

~~~

## 远程仓库

### github

创建SSH key

~~~
$ ssh-keygen -t rsa -C "1060487361@qq.com"
之后一路ENTER
~~~

会在用户主目录 c盘，user/{实际用户名}/.ssh 目录下创建id_rsa（私钥）和id_rsa.pub（公钥）两个文件，用notepad++或别的文件编辑器打开rsa.pub,并复制里面的内容。

登录GitHub,打开setting，点“SSH and GPG keys”，点“New SSH key"，将公钥粘贴到输入框中即可。

添加远程库

登录GitHub,点头像傍边+号，点“New Repository"按钮创建Git仓库。（注：不要勾选README")

把本地库跟远程库关联

~~~
$ git remote add origin git@github.com:dongxvhui/learngit.git
~~~

首次推送

~~~
$ git push -u origin master
~~~

注：会弹出一个警告，the authenticity of …… 输入yes,按回车即可。

之后的推送

~~~
$ git push origin master
~~~

从远程库克隆

~~~
$ git clone git@github.com:dongxvhui/gitskills.git
~~~

进入，查看

~~~
$ cd gitskills
$ ls
~~~

先把项目fork到自己的账号下，再clone,才能推送修改。

提交修改：pull request(等待作者审核)

### gitee

跟github 类似

获取公钥，ssh粘贴公钥，创建仓库，推送，克隆项目，拉项目。

## 分支管理

### 创建、合并分支

创建并切换到分支

~~~
$ git checkout -b dev
Switched to a new branch 'dev'
~~~

相当于两条命令

~~~
$ git branch dev
$ git checkout dev
Switched to a new branch 'dev'
~~~

查看分支

~~~
$ git branch
* dev
  master
~~~

切回主干

~~~
$ git checkout master
~~~

合并分支

~~~
$ git merge dev
Already up to date.
~~~

删除分支

~~~
$ git branch -d dev
Deleted branch dev (was a375ec5).
~~~

 ### 解决冲突

分支无法自动合并时，要先解决冲突后在提交，合并。

查看分支合并图

~~~
$ git log --graph --pretty=oneline --abbrev-commit
~~~

### 分支策略

禁用分支Fast Forward

~~~
$ git merge --no-ff -m "merge with no-ff" dev
~~~

实际开发的基本原则：

1.master分支应该是非常稳定的，不能在上面干活，仅用来发布新版本。

2.在dev上干活，发布时再将dev合并到master上。

### Bug分支

dev只add还没提交，但是需要紧急修复bug，先“储藏”保护现场

~~~
$ git stash
Saved working directory and index state WIP on dev: f6d0bce add merge
~~~

切换到需要修复bug的分支（如master或dev)

~~~
$ git checkout master
~~~

先创建临时分支

~~~
$ git checkout -b issue-101
Switched to a new branch 'issue-101'
~~~

修复bug,提交

~~~
$ git add readme.txt

$ git commit -m "fix bug 101"
[issue-101 1171dd7] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)
~~~

切回master分支，合并

~~~
$ git checkout master
$ git merge --no-ff -m "merged bug fix 101" issue-101
Merge made by the 'ort' strategy.
 readme.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

~~~

切回dev分支（略)

查看状态（略）

查看工作现场

~~~
$ git stash list
stash@{0}: WIP on dev: f6d0bce add merge
~~~

恢复现场

~~~
$ git stash apply(不删stash,须手动git stash drop 删除)
$ git stash pop（恢复后删除stash)
~~~

### Feature分支

新开一个分支

~~~
$ git checkout -b feature-vulcan
~~~

add、查看状态，切回dev（略）

删除分支

~~~~
$ git branch -d feature-vulcan
Deleted branch feature-vulcan (was f6d0bce).

强行删除
$ git branch -D feature-vulcan
~~~~

查看远程库信息

~~~
$ git remote
origin
~~~

查看远程库详细信息

~~~
$ git remote -v
origin  git@github.com:dongxvhui/learngit.git (fetch)
origin  git@github.com:dongxvhui/learngit.git (push)
~~~

推送分支（master、dev、bug、feature)

~~~
$ git push origin master

$ git push origin dev
~~~

抓取分支

~~~
$ git clone git@github.com:dongxvhui/learngit.git
~~~

多人协作的一般工作模式：

1.首先，可以试图用git push origin branch-name 推送自己的修改

2.如果推送失败，则因为远程分支比本地更新，需要先用git pull视图合并

3.如果合并有冲突，则解决冲突，并在本地提交

4，没有冲突或者冲突解决后，再用git push origin branch-name 推送

 

## 标签管理

### 创建标签

切换到要打标签的分支

~~~
$ git checkout master
~~~

打标签

~~~
$ git tag v1.0
~~~

查看标签

~~~
$ git tag
v1.0
~~~

给历史提交打标签

先查看提交日志

~~~
$ git log --pretty=oneline --abbrev-commit
36722d1 (HEAD -> master, tag: v1.0, origin/master) merged bug fix 101
1171dd7 fix bug 101
e61caad merge with no-ff
f6d0bce (dev) add merge
……
~~~

找到id

打标签

~~~
$ git tag v0.9 f6d0bce

带说明的标签
$ git tag -a v0.1 -m"version 0.1 released" 311f46b

用私钥签名标签
$ git tag -s v0.2 -m "signed version 0.2 released" f702ccb（没有gpg，失败）
~~~

查看标签

~~~
$ git tag
v0.9
v1.0
~~~

### 操作标签

删除标签

~~~
$ git tag -d v0.1
Deleted tag 'v0.1' (was cdc9647)
~~~

推送标签

~~~
推动单个：
$ git push origin v1.0
推送全部
$ git push origin --tags
~~~

删除远程标签，

先从本地删除

~~~
$ git tag -d v0.9
Deleted tag 'v0.9' (was f6d0bce)
~~~

再从远程删除

~~~
$ git push origin :refs/tags/v0.9
To github.com:dongxvhui/learngit.git
 - [deleted]         v0.9
~~~

## 个性化

### 配置全局颜色

~~~
$ git config --global color.ui true
~~~

### 忽略特殊文件

编写 .gitignore

idea版本控制忽略特殊文件干净上传

注：一定要先配置.gitignore规则，然后再将项目纳入版本控制。

​        一定要先配置.gitignore规则，然后再将项目纳入版本控制。

​        一定要先配置.gitignore规则，然后再将项目纳入版本控制。

重要的事情说三遍，否则，.gitignore不生效。

常用的.gitignore配置（针对idea，直接复制粘贴即可）

~~~~
target/
pom.xml.tag
pom.xml.releaseBackup
pom.xml.versionsBackup
pom.xml.next
release.properties
dependency-reduced-pom.xml
buildNumber.properties
.mvn/timing.properties
# https://github.com/takari/maven-wrapper#usage-without-binary-jar
.mvn/wrapper/maven-wrapper.jar

HELP.md
**/mvnw
**/mvnw.cmd

**/.mvn
**/target/

.idea

**/.gitignore
~~~~



### 配置别名

~~~
命令
$ git config --global alias.st status
$ git config --global alias.co checkout
$ git config --global alias.ci commit
$ git config --global alias.br branch
操作
$ git config --global alias.unstage 'reset HEAD'
$ git config --global alias.last 'log -1'

~~~

 lg

~~~
$ git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
~~~

查看定义的别名

~~~
git config --global -l | grep alias
~~~

取消别名

~~~
git config --global --unset alias.st(别名)
~~~

查看配置文件

~~~
$ cat .git/config
~~~

当前用户的git配置文件

~~~
$ cat .gitconfig
~~~



## 搭建Git 服务器（略）

没钱，搭不起。
