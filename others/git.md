http://os.51cto.com/art/201410/456046.htm


git checkout -b [分支名] [远程名]/[分支名]


 git remote add origin https://mengyuetao@bitbucket.org/mengyuetao/yc_dfx.git

重命名

git branch -m devel develop

git remote add origin https://mengyuetao@bitbucket.org/mengyuetao/yc.git




获取指定的分支
git pull origin r01


获取指定的分支信息到本地 + 刷新本地分支，即使 notfast forward
git fetch origin +$local_git_branch:$remote_git_branch



https://git-scm.com/book/en/v2/Git-on-the-Server-Setting-Up-the-Server
