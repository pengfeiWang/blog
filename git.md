
git add 提交暂存
git commit -a -m 'tes' 暂存 提交 一次完成
git commit -m '' --amend 《--amend 撤销上一次提交》
git log 提交日志 继续回车查看后去版本， 按 Q 退出

git reset HEAD “file-name” 从暂存区撤回工作区
git checkout -- 'file' 从版本库检出指定文件

删除
git rm -f ‘file’ 工作区 暂存区都删
git rm --cached  'file' 工作区保留， 暂存区删除

恢复
git checkout  'commitId file name' 恢复版本文件
git reset --hard 'commitId' 恢复某一版本
git reset --hard HEAD^ 回一个版本 HEAD指针向上下移动
git reset -- hard HEAD~2 回多个版本
git reflog 查看历史操作 再用checkout检出

git remote 查看远程仓库名
git remote add 改远程仓库名
git remote -v 
git push origin master 同步到远程仓库 origin 仓库 master分之

git fetch 拉取不合并
git pull 拉取合并
git merge ‘origin/master’ 合并
git diff 'master origin/master' 对比本地和远端

git branch 查看分支
git branch '分支name' 创建分支
git branch -d 'branch name' 删除分之
git branch -D 'branch name' 强制删除分之
git branch -a 查看远程分支

git push origin --delete <branchName> 删除远程分支
git push origin --delete tag <tagname> 删除远程tag

git config --global push.default simple 设置默认分支


在github上创建仓库：
Create a new repository on the command line


touch README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/BrentHuang/MyRepo.git
git push -u origin master


在本地新建一个分支： git branch Branch1
切换到你的新分支: git checkout Branch1
将新分支发布在github上： git push origin Branch1
在本地删除一个分支： git branch -d Branch1
在github远程端删除一个分支： git push origin :Branch1   (分支名前的冒号代表删除)

直接使用git pull和git push的设置
git branch --set-upstream-to=origin/master master 
git branch --set-upstream-to=origin/ThirdParty ThirdParty
git config --global push.default matching

****
否则，可以使用这种语法，推送一个空分支到远程分支，其实就相当于删除远程分支：

git push origin :<branchName>
****

***
这是删除tag的方法，推送一个空tag到远程tag：

git tag -d <tagname>
git push origin :refs/tags/<tagname>
****

git tag 查看标签
git tag ‘tag name’ 创建标签


git branch --merge 查看当前分之所合并哪些分之




####删除不存在对应远程分支的本地分支

假设这样一种情况：

我创建了本地分支b1并pull到远程分支 origin/b1；
其他人在本地使用fetch或pull创建了本地的b1分支；
我删除了 origin/b1 远程分支；
其他人再次执行fetch或者pull并不会删除这个他们本地的 b1 分支，运行 git branch -a 也不能看出这个branch被删除了，如何处理？
使用下面的代码查看b1的状态：

```
$ git remote show origin
* remote origin
  Fetch URL: git@github.com:xxx/xxx.git
  Push  URL: git@github.com:xxx/xxx.git
  HEAD branch: master
  Remote branches:
    master                 tracked
    refs/remotes/origin/b1 stale (use 'git remote prune' to remove)
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)
```    
这时候能够看到b1是stale的，使用 `git remote prune origin` 可以将其从本地版本库中去除。

更简单的方法是使用这个命令，它在fetch之后删除掉没有与远程分支对应的本地分支：

git fetch -p
重命名远程分支

在git中重命名远程分支，其实就是先删除远程分支，然后重命名本地分支，再重新提交一个远程分支。

例如下面的例子中，我需要把 devel 分支重命名为 develop 分支：

```
$ git branch -av
* devel                             752bb84 Merge pull request #158 from Gwill/devel
  master                            53b27b8 Merge pull request #138 from tdlrobin/master
  zrong                             2ae98d8 modify CCFileUtils, export getFileData
  remotes/origin/HEAD               -> origin/master
  remotes/origin/add_build_script   d4a8c4f Merge branch 'master' into add_build_script
  remotes/origin/devel              752bb84 Merge pull request #158 from Gwill/devel
  remotes/origin/devel_qt51         62208f1 update .gitignore
  remotes/origin/master             53b27b8 Merge pull request #138 from tdlrobin/master
  remotes/origin/zrong              2ae98d8 modify CCFileUtils, export getFileData
```

删除远程分支：

$ git push --delete origin devel
To git@github.com:zrong/quick-cocos2d-x.git
 - [deleted]         devel
重命名本地分支：

git branch -m devel develop
推送本地分支：

$ git push origin develop
Counting objects: 92, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (48/48), done.
Writing objects: 100% (58/58), 1.38 MiB, done.
Total 58 (delta 34), reused 12 (delta 5)
To git@github.com:zrong/quick-cocos2d-x.git
 * [new branch]      develop -> develop
然而，在 github 上操作的时候，我在删除远程分支时碰到这个错误：

$ git push --delete origin devel
remote: error: refusing to delete the current branch: refs/heads/devel
To git@github.com:zrong/quick-cocos2d-x.git
 ! [remote rejected] devel (deletion of the current branch prohibited)
error: failed to push some refs to 'git@github.com:zrong/quick-cocos2d-x.git'
这是由于在 github 中，devel 是项目的默认分支。要解决此问题，这样操作：

进入 github 中该项目的 Settings 页面；
设置 Default Branch 为其他的分支（例如 master）；
重新执行删除远程分支命令。
把本地tag推送到远程

git push --tags
获取远程tag

git fetch origin tag <tagname>