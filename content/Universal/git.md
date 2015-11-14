---
title: "git"
date: 2015-11-14 16:13:09
---
[TOC]

## 不再跟踪指定文件

```
git rm --cached <file>
```

命令说明：

- `rm --cached` : 从缓存中清除指定文件，支持正则

## 取消对文件的修改

```
git checkout -- <file>
```

命令说明：

- `checkout --` : 抛弃文件修改，返回之前的版本（会覆盖当前文件）

## 取消已经暂存的文件

```
git reset HEAD <file>
```

命令说明：

- `reset HEAD` : 撤销先前的add操作（不会覆盖当前文件）

## 合并多个commit

合并之前的三个提交，首先运行：

```
git rebase -i HEAD~3
```

或者

```
git rebase -i <commitid>
```

*注意，要合并之前的三个提交，后面应当使用你的倒数第四个提交作为参数*

然后文本编辑器将会自动打开，并出现类似如下内容：
```
pick f7f3f6d changed my name a bit
pick 310154e updated README formatting and added blame
pick a5f4a0d added cat-file

# Rebase 710f0f8..a5f4a0d onto 710f0f8
#
# Commands:
#  p, pick = use commit
#  e, edit = use commit, but stop for amending
#  s, squash = use commit, but meld into previous commit
#
# If you remove a line here THAT COMMIT WILL BE LOST.
# However, if you remove everything, the rebase will be aborted.
#
```

> 正如git的注释所说，`pick`将会采用这次commit，而`squash`将会把这个commit合并到前一个当中。

我们将后面两个`pick`修改为`squash`，现在内容如下：

```
pick f7f3f6d changed my name a bit
squash 310154e updated README formatting and added blame
squash a5f4a0d added cat-file
```

保存并退出后，git将会自动应用并产生一个新的提交，保存后三个提交就被合并成了一个。