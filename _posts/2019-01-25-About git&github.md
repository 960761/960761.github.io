

## Git和github的区别
git是一个版本控制工具  
github是一个用git做版本控制的项目托管平台。  

git是一个分布式版本管理软件，是一个可以安装的软件。  
github是一个代码托管平台，是一个网站。 

github这个网站使用git这个版本管理软件来托管代码。  

git也是有类似于客户端和服务端的，客户端就是我们使用的git bash等命令工具，而服务端是要搭建一个可以保存用户提交代码和版本信息的仓库，GitHub就是一个远程的git仓库，允许用户通过本地的git命令将本地的仓库的代码和版本信息提交到GitHub远程仓库中去。


## 集中式和分布式版本管理系统的区别 
免费开源集中式版本控制系统： CVS， SVN  
付费集中式版本控制系统：ClearCase  
分布式版本控制系统：除了Git以及促使Git诞生的BitKeeper外，还有类似Git的Mercurial和Bazaar等。这些分布式版本控制系统各有特点，最快最简单也最流行的是Git    
最早Git是在Linux上开发的，很长一段时间内，Git也只能在Linux和Unix系统上跑。不过，慢慢地有人把它移植到了Windows上。现在，Git可以在Linux、Unix、Mac和Windows这几大平台上正常运行了。

## git 参考资料：

1.	git 教程-廖雪峰
2.	《pro git》book:   
    https://git-scm.com/book/en/v2

3.	Git资源汇总：  
    https://webcache.googleusercontent.com/search?q=cache:HF2QwBZsPrwJ:https://github.com/xirong/my-git/blob/master/ixirong.com.md+&cd=8&hl=en&ct=clnk&gl=us

4.	图解git 常用命令：  
    http://marklodato.github.io/visual-git-guide/index-zh-cn.html
5.	学习git的小游戏：  
    https://learngitbranching.js.org/
6.	猴子都能懂的git教程
	
## Github 参考资料：
1.	官网：    
     https://guides.github.com/
2.	Simple training:   
    https://services.github.com/on-demand/
3.	Github for windows users video:         
    https://mva.microsoft.com/en-US/training-courses/github-for-windows-users-16749?l=KTNeW39wC_6006218965


## 常用命令（git 教程 Notes）

1.	创建版本库： 在你想要创建版本库的目录下运行 git init
2.	添加新文件或提交修改： git add file.txt; git commit –m “commit description”
 
3.	使用 git status 时刻查看仓库当前的状态， git diff  filename查看最近一次做的修改
 
4.	使用git log 查看历史记录 可以使用 –graph使得历史记录图片化表示，使用—pretty=oneline使得每一条记录单行表示。
5.	在git中，用HEAD表示当前版本，上一个版本是HEAD^，上上一个为HEAD^^，也可以使用commit id来指示要回退到哪个版本。
Command: git reset –hard HEAD^  OR  git reset –hard commitID
6.	使用git reflog用来记录每一次命令
 
 
7.	Git 原理
使用add先把需要提交的内容放到stage暂存区里面，然后使用commit将暂存区里面的内容提交。Commit只负责把暂存区里面的更改提交，如果你对文件做的修改没有放到stage，那么即便commit也不会被提交的，也即没有add即便commit也不起作用。
 
8.	撤销修改：git checkout –file, git reset HEAD filename , git reset –hard commitid
 
9.	删除文件，从版本库删除 git rm filename, git commit –m “xx”； 从工作区删除 rm filename
如果误删了工作区的文件，可以使用git checkout – filename, 注意filename前有空格
Git checkout命令就是使用版本库里的版本替换工作区的版本，因此，无论工作区修改还是删除，都可以使用 git checkout一键还原。
10.	远程仓库 github ； 从本地git往远程github推送：首先添加： git remote add origin https://github.com/960761/learngit.git，其中origin 为名字，可以更改。然后推送：git push –u origin master。以上为首次，之后推送，只需要git push origin master，即git push destination source。从远程拷贝到本地：git clone https://github.com/960761/xxx.git。
11.	分支 分支相当于平行工作空间，git中使用指针完成，所以非常快
 
创建分支： git branch dev 切换分支： git checkout dev
创建并切换： git checkout –b dev
可以使用 git branch查看当前分支，且带有星号的为当前所在的分支
 
在dev上面进行修改：
 
切换到maseter 分支： git checkout master
 
合并分支： git merge dev 将参数分支合并到当前分支上
默认是使用的fast forward的合并方式，这种方式会抹掉分支的痕迹：
 
不使用fast forward方式合并为： 
 
 
12.	分支合并
No-ff branch merge:
 
Ff branch merge:
 
从上面可以看出，当使用默认的 fast forward 时，合并分支后，分支的所有信息不再不保存，就好像这个分支从来不曾存在一样；
但如果不使用fast forward，就为这个分支生成一个新的commit，即便这个分支被删除，也会保存这个分支的改动信息。

13.	分支冲突
 
当两个分支都对文件进行了修改时，如果对不同的内容进行的修改，不会有冲突，正常合并即可；如果对相同部分进行的修改，在合并时会提示错误，打开文件后，会使用 <<<>>>给出冲突的地方，手动进行改动，解决冲突后再进行合并。

14.	分支管理： 分支功能特别强大，在进行多人协作时要充分利用分支功能。

15.	If you stuck with “END” and cannot input any command, you can 
Type any of the following 3 commands.

:q  
:z  
or  
Ctrl + z.  
To quit.
