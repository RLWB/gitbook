## git 教程

### 一、简介

Git 是一个开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。

Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。

Git 与常用的版本控制工具 CVS, Subversion 等不同，它采用了分布式版本库的方式，不必服务器端软件支持。

Git 的两个主要用途： 代码备份和版本控制，你可以逐步处理代码，在需要回滚到备份副本的过程中保存每一步的进度！

### 二、背景

那什么是版本控制系统？

如果你用 Microsoft Word 写过长篇大论，那你一定有这样的经历：

想删除一个段落，又怕将来想恢复找不回来怎么办？有办法，先把当前文件“另存为……”一个新的 Word 文件，再接着改，改到一定程度，再“另存为……”一个新文件，这样一直改下去，最后你的 Word 文档变成了这样：

![lots-of-docs](imgs\0.jpeg)

过了一周，你想找回被删除的文字，但是已经记不清删除前保存在哪个文件里了，只好一个一个文件去找，真麻烦。

看着一堆乱七八糟的文件，想保留最新的一个，然后把其他的删掉，又怕哪天会用上，还不敢删，真郁闷。

更要命的是，有些部分需要你的财务同事帮助填写，于是你把文件 Copy 到 U 盘里给她（也可能通过 Email 发送一份给她），然后，你继续修改 Word 文件。一天后，同事再把 Word 文件传给你，此时，你必须想想，发给她之后到你收到她的文件期间，你作了哪些改动，得把你的改动和她的部分合并，真困难。

于是你想，如果有一个软件，不但能自动帮我记录每次文件的改动，还可以让同事协作编辑，这样就不用自己管理一堆类似的文件了，也不需要把文件传来传去。如果想查看某次改动，只需要在软件里瞄一眼就可以，岂不是很方便？

这个软件用起来就应该像这个样子，能记录每次文件的改动：

![](imgs\git01.png)

这样，你就结束了手动管理多个“版本”的史前时代，进入到版本控制的 20 世纪。

### 三、Git 安装配置

在使用 Git 前我们需要先安装 Git。Git 目前支持 Linux/Unix、Solaris、Mac 和 Windows 平台上运行。

- windows 平台安装

  国内镜像：https://npm.taobao.org/mirrors/git-for-windows/。

  安装完成，就可以是哦那个 git 命令行工具，同时也带有图形界面的工具

  开始菜单找到 git bash，打开 git 命令窗口，就可以执行 git 操作了。

### 四、Git 配置

Git 提供了一个叫做 git config 的工具，专门用来配置或读取相应的工作环境变量。

这些环境变量，决定了 Git 在各个环节的具体工作方式和行为。这些变量可以存放在以下三个不同的地方：

- `/etc/gitconfig` 文件：系统中对所有用户都普遍适用的配置。若使用 `git config` 时用 `--system` 选项，读写的就是这个文件。
- `~/.gitconfig` 文件：用户目录下的配置文件只适用于该用户。若使用 `git config` 时用 `--global` 选项，读写的就是这个文件。
- 当前项目的 Git 目录中的配置文件（也就是工作目录中的 `.git/config` 文件）：这里的配置仅仅针对当前项目有效。每一个级别的配置都会覆盖上层的相同配置，所以 `.git/config` 里的配置会覆盖 `/etc/gitconfig` 中的同名变量。

#### 用户信息

在使用 git 前，我们需要配置下用户名和电子邮件地址，当我们提交的时候可以告诉 git 我们是谁。

```shell
$ git config --global user.name "rlwb"
$ git config --global user.email rlwb@gmail.com
```

#### 查看配置信息

```shell
$ git config --list
```

#### Git 工作流程

一般工作流程如下：

- 克隆 Git 资源作为工作目录。
- 在克隆的资源上添加或修改文件。
- 如果其他人修改了，你可以更新资源。
- 在提交前查看修改。
- 提交修改。
- 在修改完成后，如果发现错误，可以撤回提交并再次修改并提交。

![](imgs/git-process.png)

#### Git 工作区、暂存区和版本库

##### 基本概念

我们先来理解下 Git 工作区、暂存区和版本库概念：

- **工作区：**就是你在电脑里能看到的目录。
- **暂存区：**英文叫 stage 或 index。一般存放在 **.git** 目录下的 index 文件（.git/index）中，所以我们把暂存区有时也叫作索引（index）。
- **版本库：**工作区有一个隐藏目录 **.git**，这个不算工作区，而是 Git 的版本库。

下面这个图展示了工作区、版本库中的暂存区和版本库之间的关系：

![](imgs/git02.jpg)

- 图中左侧为工作区，右侧为版本库。在版本库中标记为 "index" 的区域是暂存区（stage/index），标记为 "master" 的是 master 分支所代表的目录树。
- 图中我们可以看出此时 "HEAD" 实际是指向 master 分支的一个"游标"。所以图示的命令中出现 HEAD 的地方可以用 master 来替换。
- 图中的 objects 标识的区域为 Git 的对象库，实际位于 ".git/objects" 目录下，里面包含了创建的各种对象及内容。
- 当对工作区修改（或新增）的文件执行 **git add** 命令时，暂存区的目录树被更新，同时工作区修改（或新增）的文件内容被写入到对象库中的一个新的对象中，而该对象的 ID 被记录在暂存区的文件索引中。
- 当执行提交操作（git commit）时，暂存区的目录树写到版本库（对象库）中，master 分支会做相应的更新。即 master 指向的目录树就是提交时暂存区的目录树。
- 当执行 **git reset HEAD** 命令时，暂存区的目录树会被重写，被 master 分支指向的目录树所替换，但是工作区不受影响。
- 当执行 **git rm --cached <file>** 命令时，会直接从暂存区删除文件，工作区则不做出改变。
- 当执行 **git checkout .** 或者 **git checkout -- <file>** 命令时，会用暂存区全部或指定的文件替换工作区的文件。这个操作很危险，会清除工作区中未添加到暂存区的改动。
- 当执行 **git checkout HEAD .** 或者 **git checkout HEAD <file>** 命令时，会用 HEAD 指向的 master 分支中的全部或者部分文件替换暂存区和以及工作区中的文件。这个命令也是极具危险性的，因为不但会清除工作区中未提交的改动，也会清除暂存区中未提交的改动。

#### Git 创建仓库

## git init

Git 使用 **git init** 命令来初始化一个 Git 仓库，Git 的很多命令都需要在 Git 的仓库中运行，所以 **git init** 是使用 Git 的第一个命令。

在执行完成 **git init** 命令后，Git 仓库会生成一个 .git 目录，该目录包含了资源的所有元数据，其他的项目目录保持不变。

### 使用方法

使用当前目录作为 Git 仓库，我们只需使它初始化。

```
git init
```

该命令执行完后会在当前目录生成一个 .git 目录。

使用我们指定目录作为 Git 仓库。

```
git init newrepo
```

初始化后，会在 newrepo 目录下会出现一个名为 .git 的目录，所有 Git 需要的数据和资源都存放在这个目录中。

如果当前目录下有几个文件想要纳入版本控制，需要先用 git add 命令告诉 Git 开始对这些文件进行跟踪，然后提交：

```
$ git add *.c
$ git add README
$ git commit -m '初始化项目版本'
```

以上命令将目录下以 .c 结尾及 README 文件提交到仓库中。

---

## git clone

我们使用 **git clone** 从现有 Git 仓库中拷贝项目（类似 **svn checkout**）。

克隆仓库的命令格式为：

```
git clone <repo>
```

如果我们需要克隆到指定的目录，可以使用以下命令格式：

```
git clone <repo> <directory>
```

**参数说明：**

- **repo:**Git 仓库。
- **directory:**本地目录。

比如，要克隆 Ruby 语言的 Git 代码仓库 Grit，可以用下面的命令：

```
$ git clone git://github.com/schacon/grit.git
```

执行该命令后，会在当前目录下创建一个名为 grit 的目录，其中包含一个 .git 的目录，用于保存下载下来的所有版本记录。

如果要自己定义要新建的项目目录名称，可以在上面的命令末尾指定新的名字：

```
$ git clone git://github.com/schacon/grit.git mygrit
```

## 配置

git 的设置使用 **git config** 命令。

显示当前的 git 配置信息：

```
$ git config --list
credential.helper=osxkeychain
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
core.ignorecase=true
core.precomposeunicode=true
```

编辑 git 配置文件:

```
$ git config -e    # 针对当前仓库
```

或者：

```
$ git config -e --global   # 针对系统上所有仓库
```

设置提交代码时的用户信息：

```
$ git config --global user.name "runoob"
$ git config --global user.email test@runoob.com
```

如果去掉 **--global** 参数只对当前仓库有效。

#### Git 基本操作

Git 的工作就是创建和保存你项目的快照及与之后的快照进行对比。

本章将对有关创建与提交你的项目快照的命令作介绍。

Git 常用的是以下 6 个命令：**git clone**、**git push**、**git add** 、**git commit**、**git checkout**、**git pull**，后面我们会详细介绍。

![img](imgs/git-command.jpg)

**说明：**

- workspace：工作区
- staging area：暂存区/缓存区
- local repository：或本地仓库
- remote repository：远程仓库

一个简单的操作步骤：

```
$ git init
$ git add .
$ git commit
```

- git init - 初始化仓库。
- git add . - 添加文件到暂存区。
- git commit - 将暂存区内容添加到仓库中。

### 创建仓库命令

下表列出了 git 创建仓库的命令：

| 命令        | 说明                                   |
| :---------- | :------------------------------------- |
| `git init`  | 初始化仓库                             |
| `git clone` | 拷贝一份远程仓库，也就是下载一个项目。 |

---

## 提交与修改

Git 的工作就是创建和保存你的项目的快照及与之后的快照进行对比。

下表列出了有关创建与提交你的项目的快照的命令：

| 命令         | 说明                                     |
| :----------- | :--------------------------------------- |
| `git add`    | 添加文件到仓库                           |
| `git status` | 查看仓库当前的状态，显示有变更的文件。   |
| `git diff`   | 比较文件的不同，即暂存区和工作区的差异。 |
| `git commit` | 提交暂存区到本地仓库。                   |
| `git reset`  | 回退版本。                               |
| `git rm`     | 删除工作区文件。                         |
| `git mv`     | 移动或重命名工作区文件。                 |

### 提交日志

| 命令               | 说明                                 |
| :----------------- | :----------------------------------- |
| `git log`          | 查看历史提交记录                     |
| `git blame <file>` | 以列表形式查看指定文件的历史修改记录 |

### 远程操作

| 命令         | 说明               |
| :----------- | :----------------- |
| `git remote` | 远程仓库操作       |
| `git fetch`  | 从远程获取代码库   |
| `git pull`   | 下载远程代码并合并 |
| `git push`   | 上传远程代码并合并 |

#### git add 命令

**git add** 命令可将该文件添加到暂存区。

添加一个或多个文件到暂存区：

```shell
git add [file1] [file2] ...
```

添加指定目录到暂存区，包括子目录：

```shell
git add [dir]
```

添加当前目录下的所有文件到暂存区：

```shell
git add .
```

以下实例我们添加两个文件：

```shell
$ touch README                # 创建文件
$ touch hello.php             # 创建文件
$ ls
README        hello.php
$ git status -s
M README
M hello.php
$
```

git status 命令用于查看项目的当前状态。

接下来我们执行 git add 命令来添加文件：

```
$ git add README hello.php
```

现在我们再执行 git status，就可以看到这两个文件已经加上去了。

```shell
$ git status -s
M  README
M  hello.php
$
```

新项目中，添加所有文件很普遍，我们可以使用 **git add .** 命令来添加当前项目的所有文件。

现在我们修改 README 文件：

```shell
$ vim README
```

在 README 添加以下内容：**# Runoob Git 测试**，然后保存退出。

再执行一下 git status：

```shell
$ git status -s
MM README
M  hello.php
```

**MM**状态的意思是这个文件在我们将它添加到缓存之后又有改动。改动后我们再执行 **git add .** 命令将其添加到缓存中：

```shell
$ git add .
$ git status -s
A  README
A  hello.php
```

文件修改后，我们一般都需要进行 git add 操作，从而保存历史版本。

#### Git commit 命令

前面章节我们使用 git add 命令将内容写入暂存区。

git commit 命令将暂存区内容添加到本地仓库中。

提交暂存区到本地仓库中:

```
git commit -m [message]
```

[message] 可以是一些备注信息。

提交暂存区的指定文件到仓库区：

```
$ git commit [file1] [file2] ... -m [message]
```

**-a** 参数设置修改文件后不需要执行 git add 命令，直接来提交

```
$ git commit -a
```

### 设置提交代码时的用户信息

开始前我们需要先设置提交的用户信息，包括用户名和邮箱：

```
$ git config --global user.name 'runoob'
$ git config --global user.email test@runoob.com
```

如果去掉 --global 参数只对当前仓库有效。

### 提交修改

接下来我们就可以对 hello.php 的所有改动从暂存区内容添加到本地仓库中。

以下实例，我们使用 -m 选项以在命令行中提供提交注释。

```
$ git add hello.php
$ git status -s
A  README
A  hello.php
$ git commit -m '第一次版本提交'
[master (root-commit) d32cf1f] 第一次版本提交
 2 files changed, 4 insertions(+)
 create mode 100644 README
 create mode 100644 hello.php

```

现在我们已经记录了快照。如果我们再执行 git status:

```
$ git status
# On branch master
nothing to commit (working directory clean)
```

以上输出说明我们在最近一次提交之后，没有做任何改动，是一个 "working directory clean"，翻译过来就是干净的工作目录。

如果你没有设置 -m 选项，Git 会尝试为你打开一个编辑器以填写提交信息。 如果 Git 在你对它的配置中找不到相关信息，默认会打开 vim。屏幕会像这样：

```
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
# modified:   hello.php
#
~
~
".git/COMMIT_EDITMSG" 9L, 257C
```

如果你觉得 git add 提交缓存的流程太过繁琐，Git 也允许你用 -a 选项跳过这一步。命令格式如下：

```
git commit -a
```

我们先修改 hello.php 文件为以下内容：

```
<?php
echo '菜鸟教程：www.runoob.com';
echo '菜鸟教程：www.runoob.com';
?>
```

再执行以下命令：

```
$ git commit -am '修改 hello.php 文件'
[master 71ee2cb] 修改 hello.php 文件
 1 file changed, 1 insertion(+)
```

#### Git diff 命令

git diff 命令比较文件的不同，即比较文件在暂存区和工作区的差异。

git diff 命令显示已写入暂存区和已经被修改但尚未写入暂存区文件对区别。

git diff 有两个主要的应用场景。

- 尚未缓存的改动：**git diff**
- 查看已缓存的改动： **git diff --cached**
- 查看已缓存的与未缓存的所有改动：**git diff HEAD**
- 显示摘要而非整个 diff：**git diff --stat**

显示暂存区和工作区的差异:

```
$ git diff [file]
```

显示暂存区和上一次提交(commit)的差异:

```
$ git diff --cached [file]
或
$ git diff --staged [file]
```

显示两次提交之间的差异:

```
$ git diff [first-branch]...[second-branch]
```

在 hello.php 文件中输入以下内容：

```
<?php
echo '菜鸟教程：www.runoob.com';
?>
```

使用 git status 查看状态：

```
$ git status -s
A  README
AM hello.php
$ git diff
diff --git a/hello.php b/hello.php
index e69de29..69b5711 100644
--- a/hello.php
+++ b/hello.php
@@ -0,0 +1,3 @@
+<?php
+echo '菜鸟教程：www.runoob.com';
+?>
```

git status 显示你上次提交更新后的更改或者写入缓存的改动， 而 git diff 一行一行地显示这些改动具体是啥。

接下来我们来查看下 **git diff --cached** 的执行效果：

```shell
$ git add hello.php
$ git status -s
A  README
A  hello.php
$ git diff --cached
diff --git a/README b/README
new file mode 100644
index 0000000..8f87495
--- /dev/null
+++ b/README
@@ -0,0 +1 @@
+*# Runoob Git 测试*
diff --git a/hello.php b/hello.php
new file mode 100644
index 0000000..69b5711
--- /dev/null
+++ b/hello.php
@@ -0,0 +1,3 @@
+ < ?php
+echo '菜鸟教程：www.runoob.com';
+?>
```

#### Git rm命令

git rm 命令用于删除文件。

如果只是简单地从工作目录中手工删除文件，运行 **git status** 时就会在 **Changes not staged for commit** 的提示。

git rm 删除文件有以下几种形式：

1、将文件从暂存区和工作区中删除：

```shell
git rm <file>
```

以下实例从暂存区和工作区中删除 runoob.txt 文件：

```shell
git rm runoob.txt 
```

如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 **-f**。

强行从暂存区和工作区中删除修改后的 runoob.txt 文件：

```shell
git rm -f runoob.txt 
```

如果想把文件从暂存区域移除，但仍然希望保留在当前工作目录中，换句话说，仅是从跟踪清单中删除，使用 **--cached** 选项即可：

```shell
git rm --cached <file>
```

以下实例从暂存区中删除 runoob.txt 文件：

```shell
git rm --cached runoob.txt
```

### 实例

删除 hello.php 文件：

```shell
$ git rm hello.php 
rm 'hello.php'
$ ls
README
```

文件从暂存区域移除，但工作区保留：

```shell
$ git rm --cached README 
rm 'README'
$ ls
README
```

可以递归删除，即如果后面跟的是一个目录做为参数，则会递归删除整个目录中的所有子目录和文件：

```shell
git rm –r * 
```

进入某个目录中，执行此语句，会删除该目录下的所有文件和子目录。



#### Git mv命令

git mv 命令用于移动或重命名一个文件、目录或软连接。

```shell
git mv [file] [newfile]
```

如果新但文件名已经存在，但还是要重命名它，可以使用 **-f** 参数：

```shell
git mv -f [file] [newfile]
```

我们可以添加一个 README 文件（如果没有但话）：

```shell
$ git add README 
```

然后对其重命名:

```shell
$ git mv README  README.md
$ ls
README.md
```

#### Git分支管理

几乎每一种版本控制系统都以某种形式支持分支。使用分支意味着你可以从开发主线上分离开来，然后在不影响主线的同时继续工作。

有人把 Git 的分支模型称为**必杀技特性**，而正是因为它，将 **Git** 从版本控制系统家族里区分出来。

创建分支命令：

```
git branch (branchname)
```

切换分支命令:

```
git checkout (branchname)
```

当你切换分支的时候，Git 会用该分支的最后提交的快照替换你的工作目录的内容， 所以多个分支不需要多个目录。

合并分支命令:

```
git merge 
```

你可以多次合并到统一分支， 也可以选择在合并之后直接删除被并入的分支。

开始前我们先创建一个测试目录：

```
$ mkdir gitdemo
$ cd gitdemo/
$ git init
Initialized empty Git repository...
$ touch README
$ git add README
$ git commit -m '第一次版本提交'
[master (root-commit) 3b58100] 第一次版本提交
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 README
```

------

## Git 分支管理

### 列出分支

列出分支基本命令：

```
git branch
```

没有参数时，**git branch** 会列出你在本地的分支。

```
$ git branch
* master
```

此例的意思就是，我们有一个叫做 **master** 的分支，并且该分支是当前分支。

当你执行 **git init** 的时候，默认情况下 Git 就会为你创建 **master** 分支。

如果我们要手动创建一个分支。执行 **git branch (branchname)** 即可。

```
$ git branch testing
$ git branch
* master
  testing
```

现在我们可以看到，有了一个新分支 **testing**。

当你以此方式在上次提交更新之后创建了新分支，如果后来又有更新提交， 然后又切换到了 **testing** 分支，Git 将还原你的工作目录到你创建分支时候的样子。

接下来我们将演示如何切换分支，我们用 git checkout (branch) 切换到我们要修改的分支。

```
$ ls
README
$ echo 'runoob.com' > test.txt
$ git add .
$ git commit -m 'add test.txt'
[master 3e92c19] add test.txt
 1 file changed, 1 insertion(+)
 create mode 100644 test.txt
$ ls
README        test.txt
$ git checkout testing
Switched to branch 'testing'
$ ls
README
```

当我们切换到 **testing** 分支的时候，我们添加的新文件 test.txt 被移除了。切换回 **master** 分支的时候，它们有重新出现了。

```
$ git checkout master
Switched to branch 'master'
$ ls
README        test.txt
```

我们也可以使用 git checkout -b (branchname) 命令来创建新分支并立即切换到该分支下，从而在该分支中操作。

```
$ git checkout -b newtest
Switched to a new branch 'newtest'
$ git rm test.txt 
rm 'test.txt'
$ ls
README
$ touch runoob.php
$ git add .
$ git commit -am 'removed test.txt、add runoob.php'
[newtest c1501a2] removed test.txt、add runoob.php
 2 files changed, 1 deletion(-)
 create mode 100644 runoob.php
 delete mode 100644 test.txt
$ ls
README        runoob.php
$ git checkout master
Switched to branch 'master'
$ ls
README        test.txt
```

如你所见，我们创建了一个分支，在该分支的上移除了一些文件 test.txt，并添加了 runoob.php 文件，然后切换回我们的主分支，删除的 test.txt 文件又回来了，且新增加的 runoob.php 不存在主分支中。

使用分支将工作切分开来，从而让我们能够在不同开发环境中做事，并来回切换。

### 删除分支

删除分支命令：

```
git branch -d (branchname)
```

例如我们要删除 testing 分支：

```
$ git branch
* master
  testing
$ git branch -d testing
Deleted branch testing (was 85fc7e7).
$ git branch
* master
```

### 分支合并

一旦某分支有了独立内容，你终究会希望将它合并回到你的主分支。 你可以使用以下命令将任何分支合并到当前分支中去：

```
git merge
$ git branch
* master
  newtest
$ ls
README        test.txt
$ git merge newtest
Updating 3e92c19..c1501a2
Fast-forward
 runoob.php | 0
 test.txt   | 1 -
 2 files changed, 1 deletion(-)
 create mode 100644 runoob.php
 delete mode 100644 test.txt
$ ls
README        runoob.php
```

以上实例中我们将 newtest 分支合并到主分支去，test.txt 文件被删除。

合并完后就可以删除分支:

```
$ git branch -d newtest
Deleted branch newtest (was c1501a2).
```

删除后， 就只剩下 master 分支了：

```
$ git branch
* master
```

### 合并冲突

合并并不仅仅是简单的文件添加、移除的操作，Git 也会合并修改。

```
$ git branch
* master
$ cat runoob.php
```

首先，我们创建一个叫做 change_site 的分支，切换过去，我们将 runoob.php 内容改为:

```
<?php
echo 'runoob';
?>
```

创建 change_site 分支：

```
$ git checkout -b change_site
Switched to a new branch 'change_site'
$ vim runoob.php
$ head -3 runoob.php
<?php
echo 'runoob';
?>
$ git commit -am 'changed the runoob.php'
[change_site 7774248] changed the runoob.php
 1 file changed, 3 insertions(+)
 
```

将修改的内容提交到 change_site 分支中。 现在，假如切换回 master 分支我们可以看内容恢复到我们修改前的(空文件，没有代码)，我们再次修改 runoob.php 文件。

```
$ git checkout master
Switched to branch 'master'
$ cat runoob.php
$ vim runoob.php    # 修改内容如下
$ cat runoob.php
<?php
echo 1;
?>
$ git diff
diff --git a/runoob.php b/runoob.php
index e69de29..ac60739 100644
--- a/runoob.php
+++ b/runoob.php
@@ -0,0 +1,3 @@
+<?php
+echo 1;
+?>
$ git commit -am '修改代码'
[master c68142b] 修改代码
 1 file changed, 3 insertions(+)
```

现在这些改变已经记录到我的 "master" 分支了。接下来我们将 "change_site" 分支合并过来。

```
$ git merge change_site
Auto-merging runoob.php
CONFLICT (content): Merge conflict in runoob.php
Automatic merge failed; fix conflicts and then commit the result.

$ cat runoob.php     # 代开文件，看到冲突内容
<?php
<<<<<<< HEAD
echo 1;
=======
echo 'runoob';
>>>>>>> change_site
?>
```

我们将前一个分支合并到 master 分支，一个合并冲突就出现了，接下来我们需要手动去修改它。

```
$ vim runoob.php 
$ cat runoob.php
<?php
echo 1;
echo 'runoob';
?>
$ git diff
diff --cc runoob.php
index ac60739,b63d7d7..0000000
--- a/runoob.php
+++ b/runoob.php
@@@ -1,3 -1,3 +1,4 @@@
  <?php
 +echo 1;
+ echo 'runoob';
  ?>
```

在 Git 中，我们可以用 git add 要告诉 Git 文件冲突已经解决

```
$ git status -s
UU runoob.php
$ git add runoob.php
$ git status -s
M  runoob.php
$ git commit
[master 88afe0e] Merge branch 'change_site'
```

现在我们成功解决了合并中的冲突，并提交了结果。



#### Git 查看提交历史

Git 提交历史一般常用两个命令：

- **git log** - 查看历史提交记录。
- **git blame <file>** - 以列表形式查看指定文件的历史修改记录。
- ### git log

- 在使用 Git 提交了若干更新之后，又或者克隆了某个项目，想回顾下提交历史，我们可以使用 **git log** 命令查看。

- 针对我们前一章节的操作，使用 **git log** 命令列出历史提交记录如下：

- ```shell
  $ git log
  commit d5e9fc2c811e0ca2b2d28506ef7dc14171a207d9 (HEAD -> master)
  Merge: c68142b 7774248
  Author: runoob <test@runoob.com>
  Date:   Fri May 3 15:55:58 2019 +0800
  
      Merge branch 'change_site'
  
  commit c68142b562c260c3071754623b08e2657b4c6d5b
  Author: runoob <test@runoob.com>
  Date:   Fri May 3 15:52:12 2019 +0800
  
      修改代码
  
  commit 777424832e714cf65d3be79b50a4717aea51ab69 (change_site)
  Author: runoob <test@runoob.com>
  Date:   Fri May 3 15:49:26 2019 +0800
  
      changed the runoob.php
  
  commit c1501a244676ff55e7cccac1ecac0e18cbf6cb00
  Author: runoob <test@runoob.com>
  Date:   Fri May 3 15:35:32 2019 +0800
  ```

- 我们可以用 --oneline 选项来查看历史记录的简洁的版本。

- ```shell
  $ git log --oneline
  $ git log --oneline
  d5e9fc2 (HEAD -> master) Merge branch 'change_site'
  c68142b 修改代码
  7774248 (change_site) changed the runoob.php
  c1501a2 removed test.txt、add runoob.php
  3e92c19 add test.txt
  3b58100 第一次版本提交
  ```

- 这告诉我们的是，此项目的开发历史。

- 我们还可以用 --graph 选项，查看历史中什么时候出现了分支、合并。以下为相同的命令，开启了拓扑图选项：

- ```
  *   d5e9fc2 (HEAD -> master) Merge branch 'change_site'
  |\  
  | * 7774248 (change_site) changed the runoob.php
  * | c68142b 修改代码
  |/  
  * c1501a2 removed test.txt、add runoob.php
  * 3e92c19 add test.txt
  * 3b58100 第一次版本提交
  ```

- 现在我们可以更清楚明了地看到何时工作分叉、又何时归并。

- 你也可以用 **--reverse** 参数来逆向显示所有日志。

- ```
  $ git log --reverse --oneline
  3b58100 第一次版本提交
  3e92c19 add test.txt
  c1501a2 removed test.txt、add runoob.php
  7774248 (change_site) changed the runoob.php
  c68142b 修改代码
  d5e9fc2 (HEAD -> master) Merge branch 'change_site'
  ```

- 如果只想查找指定用户的提交日志可以使用命令：git log --author , 例如，比方说我们要找 Git 源码中 Linus 提交的部分：

- ```
  $ git log --author=Linus --oneline -5
  81b50f3 Move 'builtin-*' into a 'builtin/' subdirectory
  3bb7256 make "index-pack" a built-in
  377d027 make "git pack-redundant" a built-in
  b532581 make "git unpack-file" a built-in
  112dd51 make "mktag" a built-in
  ```

- 如果你要指定日期，可以执行几个选项：--since 和 --before，但是你也可以用 --until 和 --after。

- 例如，如果我要看 Git 项目中三周前且在四月十八日之后的所有提交，我可以执行这个（我还用了 --no-merges 选项以隐藏合并提交）：

- ```
  $ git log --oneline --before={3.weeks.ago} --after={2010-04-18} --no-merges
  5469e2d Git 1.7.1-rc2
  d43427d Documentation/remote-helpers: Fix typos and improve language
  272a36b Fixup: Second argument may be any arbitrary string
  b6c8d2d Documentation/remote-helpers: Add invocation section
  5ce4f4e Documentation/urls: Rewrite to accomodate transport::address
  00b84e9 Documentation/remote-helpers: Rewrite description
  03aa87e Documentation: Describe other situations where -z affects git diff
  77bc694 rebase-interactive: silence warning when no commits rewritten
  636db2c t3301: add tests to use --format="%N"
  ```

- 更多 git log 命令可查看：http://git-scm.com/docs/git-log

- ### git blame

- 如果要查看指定文件的修改记录可以使用 git blame 命令，格式如下：

- ```
  git blame <file>
  ```

- git blame 命令是以列表形式显示修改记录，如下实例：

- ```
  $ git blame README 
  ^d2097aa (tianqixin 2020-08-25 14:59:25 +0800 1) 
  db9315b0 (runoob    2020-08-25 16:00:23 +0800 2)
  ```

#### Git 标签

如果你达到一个重要的阶段，并希望永远记住那个特别的提交快照，你可以使用 git tag 给它打上标签。

比如说，我们想为我们的 runoob 项目发布一个"1.0"版本。 我们可以用 git tag -a v1.0 命令给最新一次提交打上（HEAD）"v1.0"的标签。

-a 选项意为"创建一个带注解的标签"。 不用 -a 选项也可以执行的，但它不会记录这标签是啥时候打的，谁打的，也不会让你添加个标签的注解。 我推荐一直创建带注解的标签。

```
$ git tag -a v1.0 
```

当你执行 git tag -a 命令时，Git 会打开你的编辑器，让你写一句标签注解，就像你给提交写注解一样。

现在，注意当我们执行 git log --decorate 时，我们可以看到我们的标签了：

```
*   d5e9fc2 (HEAD -> master) Merge branch 'change_site'
|\  
| * 7774248 (change_site) changed the runoob.php
* | c68142b 修改代码
|/  
* c1501a2 removed test.txt、add runoob.php
* 3e92c19 add test.txt
* 3b58100 第一次版本提交
```

如果我们忘了给某个提交打标签，又将它发布了，我们可以给它追加标签。

例如，假设我们发布了提交 85fc7e7(上面实例最后一行)，但是那时候忘了给它打标签。 我们现在也可以：

```
$ git tag -a v0.9 85fc7e7
$ git log --oneline --decorate --graph
*   d5e9fc2 (HEAD -> master) Merge branch 'change_site'
|\  
| * 7774248 (change_site) changed the runoob.php
* | c68142b 修改代码
|/  
* c1501a2 removed test.txt、add runoob.php
* 3e92c19 add test.txt
* 3b58100 (tag: v0.9) 第一次版本提交
```

如果我们要查看所有标签可以使用以下命令：

```
$ git tag
v0.9
v1.0
```

指定标签信息命令：

```
git tag -a <tagname> -m "runoob.com标签"
```

PGP签名标签命令：

```
git tag -s <tagname> -m "runoob.com标签"
```

### 远程操作命令

#### git remote

**git remote** 命用于在远程仓库的操作。

本章节内容我们将以 Github 作为远程仓库来操作，所以阅读本章节前需要先阅读关于 Github 的相关内容：[Git 远程仓库(Github)](https://www.runoob.com/git/git-remote-repo.html)。

显示所有远程仓库：

```
git remote -v
```

以下我们先载入远程仓库，然后查看信息：

$ **git clone** https:**//**github.com**/**tianqixin**/**runoob-git-test
$ **cd** runoob-git-test
$ **git remote** -v
origin https:**//**github.com**/**tianqixin**/**runoob-git-test **(**fetch**)**
origin https:**//**github.com**/**tianqixin**/**runoob-git-test **(**push**)**

**origin** 为远程地址的别名。

显示某个远程仓库的信息：

```
git remote show [remote]
```

例如：

```
$ git remote show https://github.com/tianqixin/runoob-git-test
* remote https://github.com/tianqixin/runoob-git-test
  Fetch URL: https://github.com/tianqixin/runoob-git-test
  Push  URL: https://github.com/tianqixin/runoob-git-test
  HEAD branch: master
  Local ref configured for 'git push':
    master pushes to master (local out of date)
```

添加远程版本库：

```
git remote add [shortname] [url]
```

shortname 为本地的版本库，例如：

```
# 提交到 Github
$ git remote add origin git@github.com:tianqixin/runoob-git-test.git
$ git push -u origin master
```

其他相关命令：

``` 
git remote rm name  # 删除远程仓库
git remote rename old_name new_name  # 修改仓库名
```

#### git fetch

**git fetch** 命令用于从远程获取代码库。

本章节内容我们将以 Github 作为远程仓库来操作，所以阅读本章节前需要先阅读关于 Github 的相关内容：[Git 远程仓库(Github)](https://www.runoob.com/git/git-remote-repo.html)。

该命令执行完后需要执行 git merge 远程分支到你所在的分支。

从远端仓库提取数据并尝试合并到当前分支：

```
git merge
```

该命令就是在执行 git fetch 之后紧接着执行 git merge 远程分支到你所在的任意分支。

假设你配置好了一个远程仓库，并且你想要提取更新的数据，你可以首先执行:

```
git fetch [alias]
```

以上命令告诉 Git 去获取它有你没有的数据，然后你可以执行：

```
git merge [alias]/[branch]
```

以上命令将服务器上的任何更新（假设有人这时候推送到服务器了）合并到你的当前分支。

本章节以 https://github.com/tianqixin/runoob-git-test 为例。

接下来我们在 Github 上点击 **README.md** 并在线修改它:

![img](https://www.runoob.com/wp-content/uploads/2015/03/C5A6670F-202D-4F2C-8A63-07CEA37BB67A.jpg)

然后我们在本地更新修改。

```
$ git fetch origin
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From github.com:tianqixin/runoob-git-test
   0205aab..febd8ed  master     -> origin/master
```

以上信息"0205aab..febd8ed master -> origin/master" 说明 master 分支已被更新，我们可以使用以下命令将更新同步到本地：

```
$ git merge origin/master
Updating 0205aab..febd8ed
Fast-forward
 README.md | 1 +
 1 file changed, 1 insertion(+)
```

查看 README.md 文件内容：

```
$ cat README.md 
# 菜鸟教程 Git 测试
## 第一次修改内容
```



#### git pull

**git pull** 命令用于从远程获取代码并合并本地的版本。

**git pull** 其实就是 **git fetch** 和 **git merge FETCH_HEAD** 的简写。 命令格式如下：

```
git pull <远程主机名> <远程分支名>:<本地分支名>
```

### 实例

更新操作：

```
$ git pull
$ git pull origin
```

将远程主机 origin 的 master 分支拉取过来，与本地的 brantest 分支合并。

```
git pull origin master:brantest
```

如果远程分支是与当前分支合并，则冒号后面的部分可以省略。

```
git pull origin master
```

上面命令表示，取回 origin/master 分支，再与本地的 brantest 分支合并。

上面的 pull 操作用 fetch 表示为：

以我的 https://github.com/tianqixin/runoob-git-test 为例，远程载入合并本地分支。

```
$ git remote -v  # 查看信息
origin    https://github.com/tianqixin/runoob-git-test (fetch)
origin    https://github.com/tianqixin/runoob-git-test (push)

$ git pull origin master
From https://github.com/tianqixin/runoob-git-test
 * branch            master     -> FETCH_HEAD
Already up to date.
```

上面命令表示，取回 origin/master 分支，再与本地的 master 分支合并。

#### git push



**git push** 命用于从将本地的分支版本上传到远程并合并。

**git pull** 其实就是 **git fetch** 和 **git merge FETCH_HEAD** 的简写。 命令格式如下：

```
git push <远程主机名> <本地分支名>:<远程分支名>
```

如果本地分支名与远程分支名相同，则可以省略冒号：

```
git push <远程主机名> <本地分支名>
```

### 实例

以下命令将本地的 master 分支推送到 origin 主机的 master 分支。

```
$ git push origin master
```

相等于：

```
$ git push origin master:master
```

如果本地版本与远程版本有差异，但又要强制推送可以使用 --force 参数：

```
git push --force origin master
```

删除主机但分支可以使用 --delete 参数，以下命令表示删除 origin 主机的 master 分支：

```
git push origin --delete master
```

以我的 https://github.com/tianqixin/runoob-git-test 为例，本地添加文件：

```
$ touch runoob-test.txt      # 添加文件
$ git add runoob-test.txt 
$ git commit -m "添加到远程"
master 69e702d] 添加到远程
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 runoob-test.txt

$ git push origin master    # 推送到 Github
```

将本地的 master 分支推送到 origin 主机的 master 分支。



# Git 远程仓库(Github)

Git 并不像 SVN 那样有个中心服务器。

目前我们使用到的 Git 命令都是在本地执行，如果你想通过 Git 分享你的代码或者与其他开发人员合作。 你就需要将数据放到一台其他开发人员能够连接的服务器上。

本例使用了 Github 作为远程仓库，你可以先阅读我们的 [Github 简明教程。](https://www.runoob.com/w3cnote/git-guide.html)

![img](https://www.runoob.com/wp-content/uploads/2015/03/Git-push-command.jpeg)

------

## 添加远程库

要添加一个新的远程仓库，可以指定一个简单的名字，以便将来引用,命令格式如下：

```
git remote add [shortname] [url]
```

本例以 Github 为例作为远程仓库，如果你没有 Github 可以在官网 https://github.com/注册。



由于你的本地 Git 仓库和 GitHub 仓库之间的传输是通过SSH加密的，所以我们需要配置验证信息：

使用以下命令生成 SSH Key：

```
$ ssh-keygen -t rsa -C "youremail@example.com"
```

后面的 **your_email@youremail.com** 改为你在 Github 上注册的邮箱，之后会要求确认路径和输入密码，我们这使用默认的一路回车就行。

成功的话会在 **~/** 下生成 **.ssh** 文件夹，进去，打开 **id_rsa.pub**，复制里面的 **key**。

```
$ ssh-keygen -t rsa -C "429240967@qq.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/tianqixin/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase):    # 直接回车
Enter same passphrase again:                   # 直接回车
Your identification has been saved in /Users/tianqixin/.ssh/id_rsa.
Your public key has been saved in /Users/tianqixin/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:MDKVidPTDXIQoJwoqUmI4LBAsg5XByBlrOEzkxrwARI 429240967@qq.com
The key's randomart image is:
+---[RSA 3072]----+
|E*+.+=**oo       |
|%Oo+oo=o. .      |
|%**.o.o.         |
|OO.  o o         |
|+o+     S        |
|.                |
|                 |
|                 |
|                 |
+----[SHA256]-----+
```

回到 github 上，进入 Account => Settings（账户配置）。

![img](https://www.runoob.com/wp-content/uploads/2015/03/48840BF0-992F-4CCC-A388-15CB74819D88.jpg)

左边选择 **SSH and GPG keys**，然后点击 **New SSH key** 按钮,title 设置标题，可以随便填，粘贴在你电脑上生成的 key。

![img](https://www.runoob.com/wp-content/uploads/2015/03/B0589847-A498-4415-8700-252BDE1B20C0.jpg)

![img](https://www.runoob.com/wp-content/uploads/2015/03/106AD534-A38A-47F3-88A3-B7BE3F2FEEF1.jpg)

添加成功后界面如下所示



![img](https://www.runoob.com/wp-content/uploads/2015/03/EC8F8872-091A-4CAB-90F2-616F34F350A9.jpg)

为了验证是否成功，输入以下命令：

```
$ ssh -T git@github.com
The authenticity of host 'github.com (52.74.223.119)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes                   # 输入 yes
Warning: Permanently added 'github.com,52.74.223.119' (RSA) to the list of known hosts.
Hi tianqixin! You've successfully authenticated, but GitHub does not provide shell access. # 成功信息
```

以下命令说明我们已成功连上 Github。

之后登录后点击" New repository " 如下图所示：

![img](https://www.runoob.com/wp-content/uploads/2015/03/github1.jpg)

之后在在Repository name 填入 runoob-git-test(远程仓库名) ，其他保持默认设置，点击"Create repository"按钮，就成功地创建了一个新的Git仓库：

![img](https://www.runoob.com/wp-content/uploads/2015/03/299CF000-7B6E-4BEC-B8C2-D9AEB053307B.jpg)

创建成功后，显示如下信息：

![img](https://www.runoob.com/wp-content/uploads/2015/03/1BCB4379-1A25-4C77-BB82-92B3E7185435.jpg)

以上信息告诉我们可以从这个仓库克隆出新的仓库，也可以把本地仓库的内容推送到GitHub仓库。

现在，我们根据 GitHub 的提示，在本地的仓库下运行命令：

```
$ mkdir runoob-git-test                     # 创建测试目录
$ cd runoob-git-test/                       # 进入测试目录
$ echo "# 菜鸟教程 Git 测试" >> README.md     # 创建 README.md 文件并写入内容
$ ls                                        # 查看目录下的文件
README
$ git init                                  # 初始化
$ git add README.md                         # 添加文件
$ git commit -m "添加 README.md 文件"        # 提交并备注信息
[master (root-commit) 0205aab] 添加 README.md 文件
 1 file changed, 1 insertion(+)
 create mode 100644 README.md

# 提交到 Github
$ git remote add origin git@github.com:tianqixin/runoob-git-test.git
$ git push -u origin master
```

以下命令请根据你在Github成功创建新仓库的地方复制，而不是根据我提供的命令，因为我们的Github用户名不一样，仓库名也不一样。

接下来我们返回 Github 创建的仓库，就可以看到文件已上传到 Github上：

![img](https://www.runoob.com/wp-content/uploads/2015/03/53CA927D-F36F-4A00-AFB2-5EAED05B535E.jpg)

------

## 查看当前的远程库

要查看当前配置有哪些远程仓库，可以用命令：

```
git remote
```

### 实例

```
$ git remote
origin
$ git remote -v
origin    git@github.com:tianqixin/runoob-git-test.git (fetch)
origin    git@github.com:tianqixin/runoob-git-test.git (push)
```

执行时加上 -v 参数，你还可以看到每个别名的实际链接地址。

------

## 提取远程仓库

Git 有两个命令用来提取远程仓库的更新。

1、从远程仓库下载新分支与数据：

```
git fetch
```

该命令执行完后需要执行 git merge 远程分支到你所在的分支。

2、从远端仓库提取数据并尝试合并到当前分支：

```
git merge
```

该命令就是在执行 git fetch 之后紧接着执行 git merge 远程分支到你所在的任意分支。

假设你配置好了一个远程仓库，并且你想要提取更新的数据，你可以首先执行 **git fetch [alias]** 告诉 Git 去获取它有你没有的数据，然后你可以执行 **git merge [alias]/[branch]** 以将服务器上的任何更新（假设有人这时候推送到服务器了）合并到你的当前分支。

接下来我们在 Github 上点击" README.md" 并在线修改它:

![img](https://www.runoob.com/wp-content/uploads/2015/03/C5A6670F-202D-4F2C-8A63-07CEA37BB67A.jpg)

然后我们在本地更新修改。

```
$ git fetch origin
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From github.com:tianqixin/runoob-git-test
   0205aab..febd8ed  master     -> origin/master
```

以上信息"0205aab..febd8ed master -> origin/master" 说明 master 分支已被更新，我们可以使用以下命令将更新同步到本地：

```
$ git merge origin/master
Updating 0205aab..febd8ed
Fast-forward
 README.md | 1 +
 1 file changed, 1 insertion(+)
```

查看 README.md 文件内容：

```
$ cat README.md 
# 菜鸟教程 Git 测试
## 第一次修改内容
```

------

## 推送到远程仓库

推送你的新分支与数据到某个远端仓库命令:

```
git push [alias] [branch]
```

以上命令将你的 [branch] 分支推送成为 [alias] 远程仓库上的 [branch] 分支，实例如下。

```
$ touch runoob-test.txt      # 添加文件
$ git add runoob-test.txt 
$ git commit -m "添加到远程"
master 69e702d] 添加到远程
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 runoob-test.txt

$ git push origin master    # 推送到 Github
```

重新回到我们的 Github 仓库，可以看到文件已经提交上来了：

![img](https://www.runoob.com/wp-content/uploads/2015/03/79A84530-7DC0-4D25-9F83-8776433A4C32.jpg)

------

## 删除远程仓库

删除远程仓库你可以使用命令：

```
git remote rm [别名]
```

### 实例

```
$ git remote -v
origin    git@github.com:tianqixin/runoob-git-test.git (fetch)
origin    git@github.com:tianqixin/runoob-git-test.git (push)

# 添加仓库 origin2
$ git remote add origin2 git@github.com:tianqixin/runoob-git-test.git

$ git remote -v
origin    git@github.com:tianqixin/runoob-git-test.git (fetch)
origin    git@github.com:tianqixin/runoob-git-test.git (push)
origin2    git@github.com:tianqixin/runoob-git-test.git (fetch)
origin2    git@github.com:tianqixin/runoob-git-test.git (push)

# 删除仓库 origin2
$ git remote rm origin2
$ git remote -v
origin    git@github.com:tianqixin/runoob-git-test.git (fetch)
origin    git@github.com:tianqixin/runoob-git-test.git (push)
```

#### Github

如果你是一枚Coder，但是你不知道Github，那么我觉的你就不是一个菜鸟级别的Coder，因为你压根不是真正Coder，你只是一个Code搬运工。

但是你如果已经在读这篇文章了，我觉的你已经知道Github了。

**正是Github，让社会化编程成为现实。**

### 什么是 Github?

github是一个基于git的代码托管平台，付费用户可以建私人仓库，我们一般的免费用户只能使用公共仓库，也就是代码要公开。

Github 由Chris Wanstrath, PJ Hyett 与Tom Preston-Werner三位开发者在2008年4月创办。迄今拥有59名全职员工，主要提供基于git的版本托管服务。

目前看来，GitHub这场冒险已经胜出。根据来自维基百科关于GitHub的描述，我们可以形象地看出GitHub的增长速度：

![img](https://www.runoob.com/wp-content/uploads/2014/05/github-repos.png)

<img src="imgs/github.png" style="zoom:100%;" />



### 注册账户以及创建仓库

要想使用github第一步当然是注册github账号了， github官网地址：https://github.com/。 之后就可以创建仓库了（免费用户只能建公共仓库），Create a New Repository，填好名称后Create，之后会出现一些仓库的配置信息，这也是一个git的简单教程。

### Github 安装

- [下载 git OSX 版](http://code.google.com/p/git-osx-installer/downloads/list?can=3)
- [下载 git Windows 版](http://msysgit.github.io/)
- [下载 git Linux 版](http://book.git-scm.com/2_installing_git.html)

### 配置Git

首先在本地创建`ssh key；`

```
$ ssh-keygen -t rsa -C "your_email@youremail.com"
```

后面的`your_email@youremail.com`改为你在github上注册的邮箱，之后会要求确认路径和输入密码，我们这使用默认的一路回车就行。成功的话会在`~/`下生成`.ssh`文件夹，进去，打开`id_rsa.pub`，复制里面的`key`。

回到github上，进入 Account Settings（账户配置），左边选择SSH Keys，Add SSH Key,title随便填，粘贴在你电脑上生成的key。

![img](https://www.runoob.com/wp-content/uploads/2014/05/github-account.jpg)

为了验证是否成功，在git bash下输入：

```
$ ssh -T git@github.com
```

如果是第一次的会提示是否continue，输入yes就会看到：You've successfully authenticated, but GitHub does not provide shell access 。这就表示已成功连上github。

接下来我们要做的就是把本地仓库传到github上去，在此之前还需要设置username和email，因为github每次commit都会记录他们。

```
$ git config --global user.name "your name"
$ git config --global user.email "your_email@youremail.com"
```

进入要上传的仓库，右键git bash，添加远程地址：

```
$ git remote add origin git@github.com:yourName/yourRepo.git
```

后面的yourName和yourRepo表示你再github的用户名和刚才新建的仓库，加完之后进入.git，打开config，这里会多出一个remote "origin"内容，这就是刚才添加的远程地址，也可以直接修改config来配置远程地址。

### 检出仓库

执行如下命令以创建一个本地仓库的克隆版本：

```
git clone /path/to/repository 
```

如果是远端服务器上的仓库，你的命令会是这个样子：

```
git clone username@host:/path/to/repository
```

### 工作流

你的本地仓库由 git 维护的三棵"树"组成。第一个是你的 `工作目录`，它持有实际文件；第二个是 `暂存区（Index）`，它像个缓存区域，临时保存你的改动；最后是 `HEAD`，它指向你最后一次提交的结果。

你可以提出更改（把它们添加到暂存区），使用如下命令：
`git add <filename>`
`git add *`
这是 git 基本工作流程的第一步；使用如下命令以实际提交改动：
`git commit -m "代码提交信息"`
现在，你的改动已经提交到了 **HEAD**，但是还没到你的远端仓库。

![img](https://www.runoob.com/wp-content/uploads/2014/05/trees.png)

### 推送改动

你的改动现在已经在本地仓库的 **HEAD** 中了。执行如下命令以将这些改动提交到远端仓库：
`git push origin master`
可以把 *master* 换成你想要推送的任何分支。

如果你还没有克隆现有仓库，并欲将你的仓库连接到某个远程服务器，你可以使用如下命令添加：
`git remote add origin <server>`
如此你就能够将你的改动推送到所添加的服务器上去了。

### 分支

分支是用来将特性开发绝缘开来的。在你创建仓库的时候，*master* 是"默认的"分支。在其他分支上进行开发，完成后再将它们合并到主分支上。

![img](https://www.runoob.com/wp-content/uploads/2014/05/branches.png)

创建一个叫做"feature_x"的分支，并切换过去：
`git checkout -b feature_x`
切换回主分支：
`git checkout master`
再把新建的分支删掉：
`git branch -d feature_x`
除非你将分支推送到远端仓库，不然该分支就是 *不为他人所见的*：
`git push origin <branch>`

### 更新与合并

要更新你的本地仓库至最新改动，执行：
`git pull`
以在你的工作目录中 *获取（fetch）* 并 *合并（merge）* 远端的改动。
要合并其他分支到你的当前分支（例如 master），执行：
`git merge <branch>`
在这两种情况下，git 都会尝试去自动合并改动。遗憾的是，这可能并非每次都成功，并可能出现*冲突（conflicts）*。 这时候就需要你修改这些文件来手动合并这些*冲突（conflicts）*。改完之后，你需要执行如下命令以将它们标记为合并成功：
`git add <filename>`
在合并改动之前，你可以使用如下命令预览差异：
`git diff <source_branch> <target_branch>`

### 标签

为软件发布创建标签是推荐的。这个概念早已存在，在 SVN 中也有。你可以执行如下命令创建一个叫做 *1.0.0* 的标签：
`git tag 1.0.0 1b2e1d63ff`
*1b2e1d63ff* 是你想要标记的提交 ID 的前 10 位字符。可以使用下列命令获取提交 ID：
`git log`
你也可以使用少一点的提交 ID 前几位，只要它的指向具有唯一性。

### 替换本地改动

假如你操作失误（当然，这最好永远不要发生），你可以使用如下命令替换掉本地改动：
`git checkout -- <filename>`
此命令会使用 HEAD 中的最新内容替换掉你的工作目录中的文件。已添加到暂存区的改动以及新文件都不会受到影响。

假如你想丢弃你在本地的所有改动与提交，可以到服务器上获取最新的版本历史，并将你本地主分支指向它：
`git fetch origin`
`git reset --hard origin/master`

### 实用小贴士

内建的图形化 git：
`gitk`
彩色的 git 输出：
`git config color.ui true`
显示历史记录时，每个提交的信息只显示一行：
`git config format.pretty oneline`
交互式添加文件到暂存区：
`git add -i`



# Git Gitee

大家都知道国内访问 Github 速度比较慢，很影响我们的使用。

如果你希望体验到 Git 飞一般的速度，可以使用国内的 Git 托管服务——[Gitee（gitee.com）](https://gitee.com/?utm_source=remote_blog_cnjc)。

Gitee 提供免费的 Git 仓库，还集成了代码质量检测、项目演示等功能。对于团队协作开发，Gitee 还提供了项目管理、代码托管、文档管理的服务，5 人以下小团队免费。

接下来我们学习一下如何使用 Gitee。

由于我们的本地 Git 仓库和 Gitee 仓库之间的传输是通过SSH加密的，所以我们需要配置验证信息。

**1、我们先在 [Gitee](https://gitee.com/?utm_source=remote_blog_cnjc) 上注册账号并登录后，然后上传自己的 SSH 公钥。**

我们在 Git Github 章节已经生成了自己的 SSH 公钥，所以我们只需要将用户主目录下的 ~/.ssh/id_rsa.pub 文件的内容粘贴 Gitee 上。

选择右上角用户头像 -> 设置，然后选择 "SSH公钥"，填写一个便于识别的标题，然后把用户主目录下的 .ssh/id_rsa.pub 文件的内容粘贴进去：

![img](https://www.runoob.com/wp-content/uploads/2020/03/gitee1.png)

![img](https://www.runoob.com/wp-content/uploads/2020/03/gitee2.png)

成功添加后如下图所示：

![img](https://www.runoob.com/wp-content/uploads/2020/03/gitee3.png)

接下来我们创建一个项目。

点击右上角的 **+** 号，新建仓库：

![img](https://www.runoob.com/wp-content/uploads/2020/03/gitee4.png)

然后添加仓库信息：

![img](https://www.runoob.com/wp-content/uploads/2020/03/gitee5.png)

创建成功后看到如下信息：

![img](https://www.runoob.com/wp-content/uploads/2020/03/gitee6.png)

接下来我们看下连接信息：

![img](https://www.runoob.com/wp-content/uploads/2020/03/gitee7.png)

项目名称最好与本地库保持一致。

然后，我们在本地库上使用命令 **git remote add** 把它和 Gitee 的远程库关联：

```
git remote add origin git@gitee.com:imnoob/runoob-test.git
```

之后，就可以正常地用 git push 和 git pull 推送了！

如果在使用命令 git remote add 时报错：

```
git remote add origin git@gitee.com:imnoob/runoob-test.git
fatal: remote origin already exists.
```

这说明本地库已经关联了一个名叫 origin 的远程库，此时，可以先用 **git remote -v** 查看远程库信息：

```
git remote -v
origin    git@github.com:tianqixin/runoob.git (fetch)
origin    git@github.com:tianqixin/runoob.git (push)
```

可以看到，本地库已经关联了 origin 的远程库，并且，该远程库指向 GitHub。

我们可以删除已有的 GitHub 远程库：

```
git remote rm origin
```

再关联 Gitee 的远程库（注意路径中需要填写正确的用户名）：

```
git remote add origin git@gitee.com:imnoob/runoob-test.git
```

此时，我们再查看远程库信息：

```
git remote -v
origin    git@gitee.com:imnoob/runoob-test.git (fetch)
origin    git@gitee.com:imnoob/runoob-test.git (push)
```

现在可以看到，origin 已经被关联到 Gitee 的远程库了。

通过 git push 命令就可以把本地库推送到 Gitee 上。

有的小伙伴又要问了，一个本地库能不能既关联 GitHub，又关联 Gitee 呢？

答案是肯定的，因为 git 本身是分布式版本控制系统，可以同步到另外一个远程库，当然也可以同步到另外两个远程库。

使用多个远程库时，我们要注意，git 给远程库起的默认名称是 origin，如果有多个远程库，我们需要用不同的名称来标识不同的远程库。

仍然以 runoob-test 本地库为例，我们先删除已关联的名为 origin 的远程库：

```
git remote rm origin
```

然后，先关联 GitHub 的远程库：

```
git remote add github git@github.com:tianqixin/runoob-git-test.git
```

注意，远程库的名称叫 github，不叫 origin 了。

接着，再关联 Gitee 的远程库：

```
git remote add gitee git@gitee.com:imnoob/runoob-test.git
```

同样注意，远程库的名称叫 gitee，不叫 origin。

现在，我们用 git remote -v 查看远程库信息，可以看到两个远程库：

```
git remote -v
gitee    git@gitee.com:imnoob/runoob-test.git (fetch)
gitee    git@gitee.com:imnoob/runoob-test.git (push)
github    git@github.com:tianqixin/runoob.git (fetch)
github    git@github.com:tianqixin/runoob.git (push)
```

如果要推送到 GitHub，使用命令：

```
git push github master
```

如果要推送到 Gitee，使用命令：

```
git push gitee master
```

这样一来，我们的本地库就可以同时与多个远程库互相同步：

![img](https://www.runoob.com/wp-content/uploads/2020/03/gitee8.png)