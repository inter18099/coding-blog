---
title: Git-rebase教程
layout: post
category: Python
date: 2017-12-20
---


**我决定不说网上一下就可以搜索的内容。重点说一下不容易找到的，我用实践踩过的坑。**

[如何优雅地合并多个 Commit](https://github.com/Jisuanke/tech-exp/issues/13) 这篇文章，说的很清楚。但我实践后，有的地方这篇文章没说。1：rebase合并遇到冲突如何解决。2:合并后如何推送到远端，如 GitHub。



# rebase遇到冲突

如果你合并的 commit 里面，有2个 commit 修改同一个文件的同一行。 rebase 时就会被 Git 报错，因为 Git不知道采用哪个commit。

报错信息：

```
CONFLICT (add/add): Merge conflict in _posts/2017-12-20-Flask搭配SQLAlchemy配置.md
error: could not apply 3ddab3b... add a post

When you have resolved this problem, run "git rebase --continue".
If you prefer to skip this patch, run "git rebase --skip" instead.
To check out the original branch and stop rebasing, run "git rebase --abort".
```

解决的步骤：

## 1，修改冲突文件

用编辑器打开报错信息中的文件，文件可能像这样

```
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
<<<<<<< HEAD
Creating a new branch is quick & simple.
=======
Creating a new branch is quick AND simple.
>>>>>>> 12345
```

Git 用等号，即=，分割了2个commit中同一行的不同内容。上面是HEAD这个commit，下面是12345这个commit，修改这部分内容，同时删掉>>>，<<<和===符号。



## 2，add 冲突文件

```
git add .
# 如果你的工作区还有别的修改文件，就不要运行上面的命令，该用git add filename
```



## 3, 继续rebase

```
git rebase --continue
```



## 4, 有可能会再遇到冲突

如果你rebase的commit是2个以上，那么很可能还会遇到其他commit之间的冲突，遇到之后不要惊讶，当时我遇到了就很困惑，心想不是已经解决了吗？其实这是另外的commit之间的冲突，如果你想知道合并运行到哪一步，可以运行

```
git status
```

如果你在rebase的中途，它会显示：

```
➜  inter18099.github.io git:(6e070d6) ✗ git status
interactive rebase in progress; onto b95287b
Last commands done (2 commands done):
   pick 6e070d6 add a post
   s 3ddab3b add a post
Next commands to do (5 remaining commands):
   s 65e5f8e update
   s f571fec update
  (use "git rebase --edit-todo" to view and edit)
You are currently rebasing branch 'master' on 'b95287b'.
  (fix conflicts and then run "git rebase --continue")
  (use "git rebase --skip" to skip this patch)
  (use "git rebase --abort" to check out the original branch)

Unmerged paths:
  (use "git reset HEAD <file>..." to unstage)
  (use "git add <file>..." to mark resolution)

	both added:      "_posts/2017-12-20-Flask\346\220\255\351\205\215SQLAlchemy\351\205\215\347\275\256.md"

no changes added to commit (use "git add" and/or "git commit -a")
```

显示已经合并了2个了，还剩5个待合并。

**所以基本上，就是重复一个循环：rebase continue—遇到冲突—解决冲突—git add—rebase continue。**

到最后终端显示：

```
Successfully rebased and updated refs/heads/master.
```

代表rebase成功。不过不要高兴的太早，有对应远程仓库的同学。这时如果你运行一下，git push/ git pull。你刚被合并的commit就会又回到你温暖的怀抱。😂。下面就讨论这个。



#  合并后如何推送到远端，如 GitHub

简单说就一个命令：

```
git push -f origin master:master
```

强制推送到远端。原理我也不明白，但这管事。在stackoverflow的[这个网页](https://stackoverflow.com/questions/22654056/how-to-push-to-remote-after-a-git-rebase)上有讨论，我看不太懂，喜欢较真的同学可以去观摩一下。