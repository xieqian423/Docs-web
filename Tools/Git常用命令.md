# Git常用命令

## 常用命令
* $ git init 创建版本库
* $ git add  [file] 添加文件，可反复多次使用，添加多个文件；
    每次修改，如果不add到暂存区，那就不会加入到commit中
* $ git commit -m [说明] 提交到版本库
* $ git reset --hard HEAD^ 回退 （HEAD^回退到上个版本，HEAD^^回退到上上个版本，HEAD~100回退到上100个版本）
* $ git log 查看提交历史,显示从最近到最远的提交日志
* $ git diff [file] 查看修改，比对
* $ git reflog 查看命令日志，可以查看提交过的所有版本
* $ git reset --hard [commit-id] 用于回退到某个版本，根据id查找
* $ git status  随时掌握工作区的状态

* $ git add命令把文件添加进去,实际上就是把要提交的所有修改放到暂存区（Stage），
执行git commit就可以一次性把暂存区的所有修改提交到分支

## 远程同步
* $ git config --list  查看当前用户信息
>>>>>>>>>第一个要配置的是你个人的用户名称和电子邮件地址:
1. $ git config --global user.name "xianbai” 配置用户名
2. $ git config --global user.email xianbai@example.com 配置邮箱
3. 用了 --global 选项，那么更改的配置文件就是位于你用户主目录下的那个，以后你所有的项目都会默认使用这里配置的用户信息。

* $ git remote add origin git@github.com:xieqian423/React-demos.git 在本地关联远程库,用户xieqian423下的名为React-demos仓库
* $ git push -u origin master 第一次推送master分支的所有内容，
        第一次加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，
        还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。
* $ git push origin master 以后每次将本地master分支的最新修改推送至GitHub
* $ git clone git@github.com:xieqian423/React-demos.git 克隆一个仓库到本地

GitHub还可以使用https://github.com/xieqian423/React-demos.git这样的地址
Git支持多种协议，包括https，默认的git://使用ssh，但通过ssh支持的原生git协议速度最快

* $ git checkout -b [dev] 创建dev分支，然后切换到dev分支 ，相当于

    $ git branch dev
    $ git checkout dev

* $ git branch  列出所有分支，当前分支前面会标一个*号，可以在当前分支上提交

* $ git checkout [master] 切换回master分支
* $ git merge [dev] 用于合并指定分支到当前分支
如果合并时残生冲突，需要手动解决再提交
* $ git merge --no-ff -m "merge with no-ff" dev   --no-ff参数，表示禁用Fast forward

* $ git branch -d [dev] 删除dev分支
    要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除
* $ git log --graph命令可以看到分支合并图


## BUG修复
修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；
当手头工作没有完成时，先git stash一下保存工作现场，然后去修复bug，修复后，再git stash pop，回到工作现场。

多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash，
    $ git stash apply stash@{0}


## 多人协作

1. 首先，可以试图用git push origin branch-name推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！
5. 如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream branch-name origin/branch-name。


## 标签
命令git tag <name>用于新建一个标签，默认为HEAD，也可以指定一个commit id；

git tag -a <tagname> -m "blablabla..."可以指定标签信息；

git tag -s <tagname> -m "blablabla..."可以用PGP签名标签；

命令git tag可以查看所有标签。


## Git与SVN
1. git时分布式的，svn是集中式
2. svn没有暂存区的概念
3. Git的分支，无论创建、切换和删除分支，Git在1秒钟之内就能完成