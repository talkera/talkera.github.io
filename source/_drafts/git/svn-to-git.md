已有的知识为我们提供经验，也为我们带来习惯性思维的束缚。

从SVN转向Git的总是不免习惯性在Git中找SVN的对标，我本人就是这样。因此特地总结了一份概论。

## svn和git设计本意
了解设计本意，才能更容易理解svn和git的区别，更轻松过度。

svn是为了解决团队开发的代码版本控制问题。所以svn采用的是一个中心仓库服务器，所有人都向一个仓库提交。

当然git也能解决团队开发的版本控制问题，但不仅如此，它还要解决**自己代码版本控制**，以及**非团队协作开发**的版本控制问题。更确切的说git更适合解决后面两个问题，只是捎带解决了第一个问题。

## 仓库的区别
svn的仓库在远程的中心服务器，而git的仓库一般就在代码目录中。git仓库创建非常便捷随意
```
cd /path/to/my/project
git init
```
这就完成了仓库的创建，目录下会生成一个`.git`的目录，这个目录跟svn在远程服务器的仓库功能是类似的，用来管理代码版本的。然后就可以完成个人代码的版本控制了。

> svn本地也可以搭建仓库，然而一般用途还是远程服务器。

### git支持仓库间通信
svn只允许用户和中心服务器通信：下载（checkout），提交（commit），更新（update）。**但git支持仓库间通信，这是git运行的根基之一。**

从远程仓库clone下来的代码，**会包含两个库，自己的库和远程库`origin`**（origin是仓库的别名）
```bash
git remote -v #查看远程仓库
origin https://github.com/path/to/project.git
```

本地的仓库也可以主动添加远程仓库。
```bash
git remote add Tyler https://github.com/tyler/project.git
```

这样就可以与远程仓库通信：
```bash
#将本地代码推送到远程仓库
git push origin
git push Tyler

#将远程仓库的更新同步到本地
git fetch origin
git fetch Tyler
```

本地有两个库，fetch命令同步的**只是远程库映射在本地的 origin 和 Tyler.**。如果需要将origin或Tyler的库与自己的代码合并，需要进行merge或rebase操作。

> git与仓库的通信

## git的团队开发模式
团队工作，中心仓库是不可避免的。svn本身就是靠中心仓库，而git是用一个不含代码的仓库充当中心仓库。
```bash
git init --bare project.git #创建一个不含代码的裸仓库
#导出一个不含代码的裸仓库
```
然后，团队的每个成员都要通过clone或者添加远程仓库的方式，将本地仓库和远程仓库关联起来。

### 提交到中心仓库
每个成员在本地开发，本地commit。然后将本地仓库push到远程仓库
```bash
git add newFile.txt modify.txt
git commit -m 'my commit'
git push origin
```
**svn的`commit`是直接提交到远程仓库的，`git commit`只是提交到本地仓库。`push`完成的才是svn的`commit`的操作。**

### 从中心服务器更新
上面仓库间通信讲到远程仓库与本地的更新要分两步，`fetch+merge` 才是完成了svn的`update`操作。为方便操作，git将两个操作合并成一个命令`git pull`.

svn在commit前需要保证本地代码是最新的，git同样要求push之前本地的仓库中必须包含中心仓库的最新提交，否则就要先pull一下：fetch到origin，然后将本地代码与origin进行合并.

然而git pull和svn update的效果并不完全相同。因为git的合并有两种方式：merge或rebase。

## 非团队协作开发
这是开源项目使用的开发方式。开源项目虽然代码开源，作者允许别人自由下载（clone）但是不能允许他们随意提交。git加入了一个远程分支机制，将其他成员的仓库中可以将添加远程分支。
```bash
git remote add Tom https://github.com/Tom/project.git
git fetch Tom
git diff
```
