# My Git Learning Note

Linux安装Git：

1、输入git，查看系统是否安装git

2、通过sudo apt-get install git安装git

Windows配置Git：

git config --global user.name "Your Name"

git config --global user.eamil "Your email"

PS: --global表示是对所有的git仓库都进行此设置

创建版本库：

git init 改变当前目录编程git可以管理的仓库

git add 把文件添加到仓库

git commit 把文件提交到仓库 git commit -m "message" 后面是备注

git status 查看仓库当前的状态

git diff  "filename"   查看文件变化

git log 显示从近到远的提交日志  --pretty=oneline

HEAD表示当前版本，上一个版本就是HEAD^，往前100个版本是HEAD~100.

使用git reset来回退版本  git reset --hard "version"

git reflog 记录每一次命令

git remote add origin "git" 把当前版本库和远程库连接起来

关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容

git checkout -b "branch-name" 创建并切换到新建分支

git branch "branch-name" 创建新分支

git checkout "branch-name" 切换到分支

git merge用于合并指定分支到当前分支

git branch -d "branch-name" 删除分支

git merge --no--ff 非fastforward模式合并分支

git stash 存储工作现场，git stash pop回到现场

git stash list 查看所有stash，git stash apply stash@{num}

git stash apply 回复后并不删除  git stash drop 删除

如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除 

要查看远程库的信息，用`git remote`  或者git remote -v

git push "branch-name"    origin代表远端

git pull 从远端更新本地

本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>` 

这就是git rebase操作的特点：把分叉的提交历史“整理”成一条直线，看上去更直观。缺点是本地的分叉提交已经被修改过了。 

git tag "tag-name" 打标签

git tag 查看所有标签   标签按照字母排序

git show "tagname" 查看标签信息

还可以创建带有说明的标签，用`-a`指定标签名，`-m`指定说明文字：

```
$ git tag -a v0.1 -m "version 0.1 released" 1094adb
```

tag 和commit挂钩

git tag -d "tag-name" 删除标签

git push origin "tag-name" 推送某个标签到远程

git config --global color.ui true git适当的显示不同颜色

