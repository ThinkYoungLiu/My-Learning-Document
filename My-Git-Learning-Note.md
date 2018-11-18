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

git log 显示从近到远的提交日志  --preety=oneline

HEAD表示当前版本，上一个版本就是HEAD^，往前100个版本是HEAD~100.

使用git reset来回退版本  git reset --hard "version"

git reflog 记录每一次命令

git remote add origin "git" 把当前版本库和远程库连接起来

关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容

