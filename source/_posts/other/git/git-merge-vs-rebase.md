---
title: Git合并 merge还是rebase
date: 2018-04-05
categories: 其他
tags: [Git]
---
git的合并本质上是分支的合并。可以是同一个仓库下分支的合并，也可以是本地仓库的分支和远程仓库分支的合并。一般情况下不指定分支，都是默认操作`master`分支。

git合并有两种方式`merge`和`rebase`。

<!--more-->

以如下场景为例：版本`1.0`发布后，A在本地开发，提交了版本`a1` a2后准备`push`。此时B已经将自己提交的版本`b1``push`了，因此A需要先`pull`，再`push`。

`pull`有两种操作：`pull`和`pull --rebase` 分布表示两种合并方式：`merge`和`rebase`.

> 本文中两种结果的箭头方向可能与网上其他解释中的方向相反。因为本文用箭头表示提交顺序，而非用箭头指向上游仓库。私以为表示提交顺序更容易理解。

这是`merge`的结果：
{% mermaid %}
graph LR;
    A(版本1.0)-->B(版本a1)
    B-->C(版本a2)
    A-->M(版本b1)
    C-->X(版本ab)
    M-->X
{% endmermaid %}
合并之后可以很清晰的看出`b1`是基于`1.0`改动的，`a1`也是基于`1.0`改动的，并且基于`a1`又产生了`a2`改动，最后将两个改动合并。

这是`rebase`的结果：
{% mermaid %}
graph LR
A(版本1.0)-->M(版本b1)
M-->B(版本a1)
B-->C(版本a2)
C-->X(版本ab)
{% endmermaid %}
`rebase`直译为变基——意思是改变基于的版本。

在这个场景下的体现是，本来`1.0`是`base`，`a1`是基于`1.0`的改动，`rebase`后`b1`变成了`base`，`a1`的改动就成了基于`b1`的了。**相当于有了一个追加的概念。**

所以`git pull --rebase`的操作实际上是:
```bash
git fetch origin
git rebase origin
```
这样的合并，更像svn的合并。

**merge是将第一参数合并进来，而rebase是将第一个参数设置为base**
```bash
git checkout master
git merge newFeature #将newFeature合并到master

git checkout master
git rebase origin/master master #将远程仓库的master分支作为base，把本地master追加上去
git checkout newFeature
git rebase master #将master分支设为base，把newFeature的修改追加到master上
```

## 合并方式的选择
merge后的日志会准确保留发生过的一切，而rebase允许我们将日志修饰成“项目的构建过程”。

rebase适用场景：
- 在同一个分支上与团队其他成员代码的合并


merge适用场景：
- 与修复bug分支的合并
- 与功能分支的合并
- 开源项目与其他贡献者的合并


## rebase解决合并冲突
```bash
git rebase
# 遇到冲突后需要手动编辑代码解决冲突。解决完成后通过git add标识冲突被解决
git add

# 然后可以继续rebase
git rebase --continue
# 或者撤销rebase还原原本状态
git rebase --abort
# 如果没什么需要处理的，可以跳过
git rebase --skip
```