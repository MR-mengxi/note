# git

> 获取git项目仓库的两种方式

1. 将没有进行版本控制的本地目录转换为git仓库（git init）
2. 从其它服务器克隆一个已经存在的git仓库

这两种方式都会在本地创建一个```.git```的文件夹

```yaml
git add命令指定所需的文件进行跟踪
git add README.md
# git add 会把文件放入暂存区
```

克隆现有仓库的命令：

```yaml
git clone <url>
```

如果想自定义本地仓库的名字，可以通过参数进行配置：

```yaml
git clone <url> 自定义文件名
```

检查当前文件状态的命令：

```yaml
git status
Changes to be committed:
# 这一行下面的文件，说明是暂存状态
Changes not staged for commit:
# 这一行下面的文件，说明已经跟踪的文件做了修改，还未放到暂存区

git status -s 或 git status --short可以使输出内容更加简洁：
A  lib/git.rb
M  lib/simplegit.rb
?? LICENSE.txt
# ??:说明是新添加的未跟踪文件
# A:说明是新添加到暂存区的文件
# M:说明是修改过的文件
```



> 忽略文件：

一些不想被git管理的文件，也不希望出现在未跟踪文件列表的文件，可以写入.gitignore文件
.gitignore的格式规范如下：

- 所有空行或者以#开头的行都会被git忽略。
- 可以使用标准的 glob 模式匹配，它会递归地应用在整个工作区中。
- 匹配模式可以以（`/`）开头防止递归。
- 匹配模式可以以（`/`）结尾指定目录。
- 要忽略指定模式以外的文件或目录，可以在模式前加上叹号（`!`）取反。



> git diff

要查看修改后尚未暂存的文件修改了哪些部分，使用```git diff```

要查看已暂存将要添加到下次提交里的内容，可以使用```git diff --staged```，也可以使用```git diff --cached```。



> 提交更新

```yaml
git commit -m '提交信息'
```



> 跳过使用暂存区

```yaml
git commit -a -m '提交信息'
# 这样会跳过git add步骤
```



> 移除文件

从暂存区删除，使用```git rm```

```yaml
git rm <file>
# 如果只是手动的删除文件，运行git status时会在Changes not staged for commit（未暂存）看到
强制删除：git rm -f
```

当想把文件从git仓库删除（不让git继续跟踪），但希望保留在本地磁盘中，忘记添加```.gitignore```文件，使用```--cached```选项。

```yaml
git rm --cached <file>
```



> 移动文件

在git中对文件改名，可以使用如下命令：

```yaml
git mv <oldfile> <newfile>
# 其实运行git mv相当于运行了下面三条命令
# mv <oldfile> <newfile>
# git rm <oldfile>
# git add <newfile>
```



> 查看提交历史

当提交多次更新，或者克隆一个项目时，想查看提交历史，可以使用```git log```命令

```yaml
1. git clone https://github.com/MR-mengxi/note.git
2. git log
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    changed the version number

commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700

    removed unnecessary test

commit a11bef06a3f659402fe7563abf99ad00de2209e6
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 10:31:28 2008 -0700

    first commit
# 不传入任何参数时，git log会按照时间的先后顺序列出所有的提交，最近的更新排在最上面    
```

git log一些常用的选项：
1. -p 或 --patch：显示每次提交的差异，可以控制日志的输出数量，例如用-2来显示最近的两次提交

```yaml
git log -p -2
```

2. --stat：查看每次提交的简略统计信息（比如：所有被修改过的文件，有多少文件被修改了以及被修改过的文件哪些行被移除或是添加了）

```yaml
git log --stat
```

3. --pretty：使用不同于默认格式的方式展示提交历史，还有一些内建子选项可以选择（oneline、short、full、fuller等）

```yaml
git log --pretty=oneline
ca82a6dff817ec66f44342007202690a93763949 changed the version number
085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7 removed unnecessary test
a11bef06a3f659402fe7563abf99ad00de2209e6 first commit

还可以使用format定制显示格式：
git log --pretty-fotmat: "%h - %an, %ar : %s"
```

git log的常用选项：

```yaml

```



> 撤销操作

当你提交后发现漏掉了一些文件没有添加，或者提交信息写错了，可以运行```--amend```命令来重新提交。

```yaml
git commit --amend
# 这个命令会将暂存区中的文件提交
```

例如：你提交后发现忘记了暂存某些修改过后的文件，可以如下操作：

```yaml
git commit -m 'initial commit'
git add <forget-file>
git commit --amend
# 最终你只会有一个提交，第二次提交将代替第一次提交的结果
# 可以理解为用一个新的提交替换旧的提交，就像是旧的提交从未存在过一样，不会出现在历史记录中，弄乱仓库历史
```



> 取消暂存的文件

当你想将两个修改的文件进行两次独立的提交时，却不小心输入```git add *```暂存了两个，如果想取消暂存中的一个，可以使用```git reset HEAD <file>```来取消暂存

```yaml
git add *
git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
git reset HEAD <filename>
# git reset 确实是个危险的命令，如果加上了 --hard 选项则更是如此。 然而在上述场景中，工作目录中的文件尚未修改，因此相对安全一些。
```



> 撤销对文件的修改

如果不想保留对文件的修改，想把它还原为上次的样子，可以使用```git checkout -- <file>```

```yaml
git status
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
```



## 远程仓库的使用

> 查看远程仓库

查看已经配置的远程仓库服务器，使用```git remote```命令，它会列出你指定的每一个远程服务器的简写，如果克隆的是自己的远程仓库，git会克隆仓库服务器的默认名字（origin）。

```yaml
git clone https://github.com/MR-mengxi/note.git
Cloning into 'ticgit'...
remote: Reusing existing pack: 1857, done.
remote: Total 1857 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (1857/1857), 374.35 KiB | 268.00 KiB/s, done.
Resolving deltas: 100% (772/772), done.
Checking connectivity... done.

git remote
origin
```

可以指定```-v```，会显示需要读写远程仓库使用的 Git 保存的简写与其对应的 URL

```yaml
git remote -v
origin  https://github.com/MR-mengxi/note.git (fetch)
origin  https://github.com/MR-mengxi/note.git (push)
```

如果你的远程仓库不止一个，git会将它们全部列出，例如，与几个协作者合作的，拥有多个远程仓库的仓库看起来像下面这样：

```yaml
git remote -v
bakkdoor  https://github.com/bakkdoor/grit (fetch)
bakkdoor  https://github.com/bakkdoor/grit (push)
cho45     https://github.com/cho45/grit (fetch)
cho45     https://github.com/cho45/grit (push)
defunkt   https://github.com/defunkt/grit (fetch)
defunkt   https://github.com/defunkt/grit (push)
koke      git://github.com/koke/grit.git (fetch)
koke      git://github.com/koke/grit.git (push)
origin    git@github.com:mojombo/grit.git (fetch)
origin    git@github.com:mojombo/grit.git (push)
```



> 添加远程仓库

运行```git remote add <shortname> <url>```添加一个新的远程git仓库，同时指定一个方便使用的简写。

```yaml
git remote 
origin

git remote add pb https://github.com/MR-mengxi/note.git
git remote -v
origin  https://github.com/MR-mengxi/note.git (fetch)
origin  https://github.com/MR-mengxi/note.git (push)
pb	https://github.com/paulboone/ticgit (fetch)
pb	https://github.com/paulboone/ticgit (push)
# 现在可以在命令行中使用字符串pb来代替整个url
# 如果你想拉取别的仓库中有，但你的仓库中没有的信息，可以运行git fetch pb
git fetch pb
```



> 从远程仓库中抓取与拉取

从远程仓库中获得数据，可以执行：

```yaml
git fetch <remote>
# 改命令会访问远程仓库，从中拉取所有你还没有的数据，这样你将会拥有远程仓库中所有分支的引用，可以随时合并或查看。

当使用clone命令克隆一个仓库后，命令会自动将其添加为远程仓库兵默认以origin为简写
git fetch origin
# 改命令会抓取克隆新推送的所有工作
# git fetch 命令只会将数据下载到你的本地仓库——它并不会自动合并或修改你当前的工作
```



> 推送到远程仓库

```yaml
git push <remote> <branch>
# remote：远程（通常是仓库名）
# branch：分支

当你想要将master分支推送到origin服务器时（克隆时会自动设置好那两个名字），运行下面的命令会将数据推送到远程仓库
git push origin master
# 这条命令只有在你有所克隆服务器的写入权限，并且之前没有推送过时才能生效。
# 当别人也克隆了这个仓库，并且比你先推送数据到仓库时，你的推送就会被拒绝。你必须先抓取他们的工作并将其合并到你的工作才能进行推送
```



> 查看某个远程仓库

查看远程仓库的更多信息，可以使用```git remote show <remote>```命令，

```yaml
git remote show origin
```



> 远程仓库的重命名与移除

可以运行```git remote rename```来修改远程仓库的简写名。

```yaml
git remote rename pb paul
```

如果想移除远程仓库，可以使用```git remote remove或git remote rm```

```yaml
git remote remove paul
```



## 打标签

> 列出标签





## git别名

可以通过```git config```文件来为命令设置一个别名。

```yaml
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
# 当输入git commit时，可以输入git ci 
```



## git分支

> 分支创建

创建新分支，使用```git branch```命令。

```yaml
git branch test
```

> 分支切换

切换到一个已经存在的分支，需要使用```git checkout```命令，切换到新创建的```test```分支。

```yaml
git checkout test
```



## git分支的新建与合并

> 新建分支

创建并切换到当前分支，可以添加一个```-b```的参数。

```yaml
git checkout -b <branch>
相当于
git branch <branch>
git checkout <branch>
```

