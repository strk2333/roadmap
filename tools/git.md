# Git



git init

git add filename/-A /. /--all

git commit -m ""

git status 告查看是否有文件被修改过

git log --pretty=oneline 查看提交历史，以便确定要回退到哪个版本

git diff 查看修改内容

git reflog 显示记录的命令 查看命令历史，以便确定要回到未来的哪个版本

HEAD 当前版本

git reset --hard commit_id

remote repository

git pull <remote> <branch>

git push --set-upstream origin master

git pull origin master --allow-unrelated-histories