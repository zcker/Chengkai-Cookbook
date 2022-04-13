---
cover: >-
  https://images.unsplash.com/photo-1556075798-4825dfaaf498?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHwxfHxnaXR8ZW58MHx8fHwxNjQ4NTUyOTA2&ixlib=rb-1.2.1&q=85
coverY: 0
---

# ♐ Git底层原理图

![](<../../.gitbook/assets/image (11).png>)

## 原理图个人解析

![git底层原理图](<../../.gitbook/assets/image (26).png>)

平时我们所说的Git中的工作区、暂存区、版本库（代码仓库或本地库），实际上我们能够感性的接触并看到的也就只有工作区，**它就是执行 git init 命令的那个目录。**

![工作区Git目录](<../../.gitbook/assets/image (4).png>)

当执行完 git init 后实际上我们也获得了暂存区和版本库，二者都是 .git 这个隐藏文件夹中的文件。暂存区对应着 .git/index 文件，版本库则对应 .git/objects 文件夹中各种类似 /cc/2dccf920fd035859e3b9e8ab8055d7dbe5a3b4 命名的的Git对象。这些Git对象有 blob 对象、 tree 对象、 commit 对象、 tag 对象。

这些文件以及Git对象和我平时在Git教程课中常常看的图有什么关联？

![Git教程中常用图](<../../.gitbook/assets/image (67).png>)

图上所谓的版本库其实就是一系列commit对象的集合（每一个commit对象为一次文件快照），而暂存区就是一个文件（index文件）用来暂存即将提交的文件列表信息。通过 git ls-files -s 可以看查看 index文件中的内容。

![index文件代表的暂存区内容](<../../.gitbook/assets/image (109).png>)

那么commit对象和其它3个Git对象有什么关系？

在逻辑上，把Git各个对象内部的指针互相引用用图画形式表现出来，在真实的 .git/bojects 中我们根本没把法用肉眼去观察 Git 各个对象之间的引用。

![Git对象互相引用](<../../.gitbook/assets/image (96).png>)

![.git 文件夹中的Git对象](<../../.gitbook/assets/image (34).png>)

通俗的讲，Git版本控制就是：通过各个Git对象的内部指针和一些存储引用的文件来构成一个Git对象网络。

## 参考资料

* [https://www.bilibili.com/read/cv8666028?from=articleDetail](https://www.bilibili.com/read/cv8666028?from=articleDetail)
