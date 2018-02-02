
## 推荐博客：<a href="https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000" target="_blank">廖雪峰Git教程

![这里写图片描述](http://img.blog.csdn.net/20180201155022278?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZ2xlbjE5NDM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 工作区 
> <p>还原被修改的文件，命令中的--很重要，没有 --，就变成了“切换到另一个分支”的命令<p>
 - `git checkout -- file`    
> <p>添加文件到暂存区<p>
 - `git add file...  `      

### 暂存区
> <p>将文件还原到工作区<p>
- `git reset file `
> <p>提交暂存区内容到master分支上，最终push到远程分支<p>
- `git commit -m 'message' `

> **注：**
git status  查看文件状态
git reset命令 既可以回退版本（commit之后），也可以把暂存区的修改回退到工作区（未commit）。当我们用HEAD时，表示最新的版本。

> （新增文件）只有`git add`并且`commit`之后才能执行`git rm file`；<p>
> 执行`git rm`命令后未执行`commit`，可通过`git reset HEAD file` 还原到工作区，再通过`git checkout -- file` 还原删除操作<p>
> 执行`git rm`命令后执行`commit`，可通过`git reset`命令既可以回退版本

>**注：**
如果在当前工作区内`test.txt`的修改(新增test.txt)未commit ,`git rm test.txt` 会报错。
另外，`git rm --cached test.txt` 会在文件系统中保留`test.txt`，但版本库中的会被删除。



**添加远程仓库：**
> 推送内容到远程库
  如果当前分支与多个主机存在追踪关系，则可以使用-u选项指定一个默认主机，这样后面就可以不加任何参数使用`git push origin master`。
-  `git remote add origin git@github.com:GlenGithub/StudyGit.git`
> 一般会提示你需要pull一下 ，建议强制执行
- `git push -u origin master` 
- `git push -u origin master -f`


**从远程库克隆：**
-  `git clone git@github.com:GlenGithub/StudyGit.git`


## 分支管理：

>每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支；每次提交，master分支都会向前移动一步。

-  `git branch  查看分支 `
- `git branch dev 创建分支dev`
- `git checkout dev 切换到dev分支上`

- `git checkout 命令加上-b参数表示创建并切换`
- `git checkout -b dev 等价于 git branch dev ，git checkout dev`

>在dev分支修改commit之后，执行
- `git checkout master 切换到master分支`
- `git merge dev  分支的工作成果快速合并到master分支上`
- `git branch -d dev   合并完后删除dev分支`

> `git merge`  命令用于合并指定分支到当前分支
如果要强制禁用`Fast forward`模式，Git就会在`merge`时生成一个新的`commit`，这样，从分支历史上就可以看出分支信息。
- `git merge --no-ff -m "merge with no-ff" dev `

## 解决冲突:

>当master,dev分支对同一个文件修改并提交时，
-  `git merge dev  提示冲突文件`
>解决冲突后，执行add，commit；
- `git log --graph --pretty=oneline --abbrev-commit  可以看到分支的合并情况`
- `git branch -d dev  删除dev分支`

## bug分支：

>正在dev上进行的工作还没有提交，这时需要修复代号101的bug
- `git stash  把当前分支的工作现场“储藏”起来，等以后恢复现场后继续工作；`
- `git checkout master  切换master分支`
- `git checkout -b bug101  在master分支创建bug101分支`
>修复完成后，切换到master分支，并完成合并，最后删除bug101分支
 - `git checkout master`
 - `git merge --no-ff -m "merged bug fix 101" bug101`
 - `git branch -d bug101`

 >解决掉bug后，切换到工作分支dev上
 - `git checkout dev`
 - ` git stash list 查看储存的工作现场`
 - `git stash apply stash@{0} 恢复现场`
 - `git stash drop stash@{0}  但是恢复后，stash内容并不删除，你需要用git stash drop来删除`
 - `git stash pop，恢复的同时把stash内容也删了`


 ## Feature分支：

 >每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。

  - `git checkout -b feature-music  #新建任务分支，`

  - `git checkout dev   #add，commit后切换到dev`


  >就在此时，接到上级命令，因经费不足，新功能必须取消！

  - `git branch -d feature-music  删除任务分支`

  >Git友情提醒，`feature-music` 分支还没有被合并，如果删除，将丢失掉修改且销毁失败，如果要强行删除，需要使用命令
  - `git branch -D feature-music`

  > **注:** 如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。


 ## 多人协作:

>当你从远程仓库克隆时，实际上Git自动把本地的master分支和远程的master分支对应起来了，并且，远程仓库的默认名称是origin

- `git remote  #查看远程库的信息`
- `git remote -v #显示更详细的信息`

>就是把该分支上的所有本地提交推送到远程库
- `git push origin master`
>如果要推送其他分支，比如dev
- `git push origin dev`

> **注：** <p>1. master分支是主分支，因此要时刻与远程同步 <p>2. dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步<p> 3. bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug <p>4. feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发

> `git clone` 从远程库clone时，默认情况下，你的小伙伴只能看到本地的master分支

> 你的小伙伴要在dev分支上开发，就必须创建远程origin的dev分支到本地，于是他用这个命令创建本地dev分支
- `git checkout -b dev origin/dev`

> 创建完dev分支后，做修改，add ，commit后需要push
- `git push origin dev`

>推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，解决办法也很简单，Git已经提示我们，先用`git pull`把最新的提交从`origin/dev`抓下来，然后，在本地合并，解决冲突，再推送
`git pull`

> `git pull`也失败了，原因是没有指定本地dev分支与远程origin/dev分支的链接
- `git branch --set-upstream dev origin/dev`

> 这回`git pull`成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的解决冲突完全一样。解决后，提交，再push

**多人协作的工作模式：**
> 1. 首先，可以试图用`git push origin branch-name`推送自己的修改<p>
>2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并<p>
>3. 如果合并有冲突，则解决冲突，并在本地提交；<p>
>4. 没有冲突或者解决掉冲突后，再用`git push origin branch-name`推送就能成功！<p>
>5. 如果`git pull`提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream branch-name origin/branch-name`。

>**注：** <p>
>1. 查看远程库信息，使用 `git remote -v` <p>
>2. 本地新建的分支如果不推送到远程，对其他人就是不可见的<p>
>3. 从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交<p>
>4. 在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致<p>
>5. 建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`<p>
>6. 从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突<p>


## 创建标签：

>在Git中打标签非常简单，首先，切换到需要打标签的分支上

 - `git checkout master #切换到master分支`
 - `git tag v1.0        #打v1.0标签`

 - `git tag             #查看所有标签`

 >默认标签是打在最新提交的`commit`上的 ，若针对特定`commit id `打标签则

 - `git log --pretty=oneline --abbrev-commit  # 显示历史提交的commit id`
 - `git tag v0.9 cc17032                      #`

 >标签不是按时间顺序列出，而是按字母排序的。可以用`git show <tagname>`查看标签信息

 >创建带有说明的标签，用`-a`指定标签名，`-m`指定说明文字
 - `git tag -a v0.1 -m "version 0.1 released" 3628164`

 >**注：**<p>
 >1. 命令`git tag <name>`用于新建一个标签，默认为`HEAD`，也可以指定一个`commit id`；<p>
 >2. `git tag -a <tagname> -m "blablabla..."`可以指定标签信息<p>
 >3. `git tag -s <tagname> -m "blablabla..."`可以用PGP签名标签<p>
 >4. 命令`git tag`可以查看所有标签<p>


 ## 操作标签：

 >如果标签打错了，也可以删除
 - `git tag -d v0.1`

 >如果要推送某个标签到远程，使用命令`git push origin <tagname>`
  - `git push origin v1.0`

  >一次性推送全部尚未推送到远程的本地标签
  - `git push origin --tags`

  >如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除
  - `git tag -d v0.9`
  >然后，从远程删除。删除命令也是`push`
  - `git push origin :refs/tags/v0.9`


  >**注：**<p>
  >1. 命令`git push origin <tagname>`可以推送一个本地标签<p>
  >2. 命令`git push origin --tags`可以推送全部未推送过的本地标签<p>
  >3. 命令`git tag -d <tagname>`可以删除一个本地标签<p>
  >4. 命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签


## 忽略特殊文件：
>在Git工作区的根目录下创建一个特殊的`.gitignore`文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件;<p>
>不需要从头写`.gitignore`文件，GitHub已经为我们准备了各种[配置文件](https://github.com/github/gitignore).<p>
>最后一步就是把`.gitignore`也提交到Git，就完成了<p>
>你想添加一个文件到Git，但发现添加不了，原因是这个文件被`.gitignore`忽略了
- `git add App.class`
>如果你确实想添加该文件，可以用-f强制添加到Git
- `git add -f App.class`

>**注：**<p>
>1. 忽略某些文件时，需要编写`.gitignore`<p>
>2. `.gitignore`文件本身要放到版本库里，并且可以对`.gitignore`做版本管理！



## 配置别名：

```
git config --global alias.st status
git st <==> git status

git config --global alias.co checkout
git co <==> git checkout

git config --global alias.ci commit
git ci <==> git commit

git config --global alias.br branch
git br <==>  git branch

git config --global alias.unstage 'reset HEAD'  #以把暂存区的修改撤销掉（unstage），重新放回工作区
git unstage file <==> git reset HEAD file

git config --global alias.last 'log -1'  #显示最后一次提交信息
git last  <==>  git log -1
```

>`--global`参数是全局参数，也就是这些命令在这台电脑的所有Git仓库下都有用。
>配置Git的时候，加上`--global`是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。

>丧心病狂lg配置:

- `git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"`

![这里写图片描述](http://img.blog.csdn.net/20180201172101135?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZ2xlbjE5NDM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

>配置文件放哪了？每个仓库的Git配置文件都放在`.git/config`文件中




