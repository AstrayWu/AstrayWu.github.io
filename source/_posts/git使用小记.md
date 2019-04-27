---
title: git使用小记
date: 2019-04-27 11:06:09
tags:
---

记录git使用过程的一些问题和方法。

## 1. 本地和远程同步

本地库要推送到远端，但这两个git库不相关，例如已有本地库，然后github上又新建了项目

- [上传本地项目到github远程仓库](https://blog.csdn.net/guotingting923/article/details/80162896)
- [git 出现 fatal: refusing to merge unrelated histories 错误](https://www.centos.bz/2018/03/git-%E5%87%BA%E7%8E%B0-fatal-refusing-to-merge-unrelated-histories-%E9%94%99%E8%AF%AF/)
- [GitHub使用部分--本地库和远程库之间的同步](https://www.jianshu.com/p/71e8dbd14b30)

方法一
可以先将远程库拉下来，然后再将本地的代码拷贝到远端库下载到本地库，然后在提交，这样相当于一次update。不过这样子不能记录原有的git记录

方法二

强制关联推送

```shell
# 本地先正常init、add和commit

# 关联远程库
$ git remote add origin https://github.com/AstrayWu/utils_python.git

# 如果远端库已经存在，则进行删除再关联
$ git remote rm origin

# 使用--allow-unrelated-hisotries进行强制关联
$ git pull origin master --allow-unrelated-hisotries

$ git push -u origin master
```

## 2. git push

- [2.5 Git 基础 - 远程仓库的使用](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E8%BF%9C%E7%A8%8B%E4%BB%93%E5%BA%93%E7%9A%84%E4%BD%BF%E7%94%A8)
- [Git push 常见用法](https://www.cnblogs.com/qianqiannian/p/6008140.html)

用`git remote`查看配置的远程库名字，一般`origin`是Git克隆的远程仓库服务器的默认名字。

```shell
$ git remote # 查看配置的远程库名字
$ git remote -v # 显示对应的URL
$ git remote add <shortname> <url> # 添加远程库并制定简称
$ git remote add pb https://github.com/paulboone/ticgit
$ git remote add pb git@github.com:paulboone/ticgit.git
$ git remote -v
$ git fetch[remote-name] # 有了简写后就可以用简写代替远程库的名字
$ git push [remote-name] [branch-name]
$ git push -u origin master 第一次同步时使用该命令
$ git push origin master
origin https://github.com/schacon/ticgit (fetch)
origin https://github.com/schacon/ticgit (push)
pb https://github.com/paulboone/ticgit (fetch)
pb https://github.com/paulboone/ticgit (push)
```