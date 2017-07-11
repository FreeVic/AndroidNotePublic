# git

	提交流程：工作区-->缓冲区-->本地仓库-->远程仓库

	master 表示默认的开发分支
	origin 默认的远程仓库版本
	HEAD   指向的是当前分支。
	HEAD^	Head的父提交


# 配置

	git config --global user.name "name"		配置用户名
	git config --global user.email "email"		配置邮箱
	git config --gobal core.autocrlf false		系统的换行符错误
	git config core.autocrlf                    crlf警告

# 创建仓库

	git init

# 添加于提交

	git add file-name
	git add . stage所有文件 unstage 就reset操作 git reset head^
	git commit -m "提交说明"
	git commit -a -m "提交说明" 		跳过缓冲区 把当前工作区已经添加到版本库的修改过的文件一次性添加缓冲区，然后提交到版本库
	git commit --amend					撤销刚刚的提交，此命令将使用当前的暂存区域快照提交。如果刚才提交完没有何改动，直接运行此命令的话，相当于有机会重新编辑提交说明，但将要提交的																文件快照和之前的一样。最终只是产生一个提交，第二个提交命令修正了第一个的提交内容。前提是你提交后没有修改
	
										使用示例：
										$ git commit -m 'initial commit' 提交了一个文件
										$ git add forgotten_file         这个文件忘记提交了，先add
										$ git commit --amend			 利用这个命令提交，将不会产生新的提交记录，而是提交到前一个版本中

# 查看

	git status	查看工作区状态
	git diff						查看当前工作区与缓冲区不同的地方
	git diff HEAD -- <file-name>    查看某一个文件当前版本和工作区有什么不同（注意空格）
	git diff --staged				查看当前工作区与最后一次版本库不同的地方

# Log

	git log  						查看每次提交： -p 选项展开显示每次提交的内容差异，
	git log -1						使用 git log -1 表示只显示最近一次
	git log --status  				仅显示简要的增改行数统计
	git log --pretty=oneline		--pretty 选项，可以指定使用完全不同于默认格式的方式展示提交历史。比如用 oneline 将每个提交放在一行显示，这在提交数很大时非常有用。
									另外还有 short，full 和 fuller 可以用，展示的信息或多或少有些不同，


	git log --graph												git提交的log视图
	git log --pretty=format:"%h %s" --graph						分支及其分化衍合情况																
	git reflog
	git log  --graph --pretty=oneline --abbrev-commit 			查看分支合并图


# 版本回滚与文件管理

	git reset --hard HEAD^      	回到上一个版本
	git reset --hard <commitId>  	回到某一个版本
	git reset HEAD <file-name>		撤销文件在缓冲区的修改
	git checkout -- <file-name>		撤销工作区的修改
	git rm <file-name>				删除文件，需要提交
	git rm --cached <file-name> 	文件已经添加到版本库，需要从版本库移除，但是不删除本地文件
	
	git mv README.md README 		移动文件 相当于下面的操作：
	git rm README.txt				删除原来的版本库中的文件
	git add README					新文件添加到版本库
	mv README.txt README			重命名和移动


# SSH KEY 如果使用ssh

	ssh-keygen -t rsa -C "your email address" 	在C:\Users\Administrator\.ssh生成私匙，邮箱地址使用初始化的邮箱  (提醒：ssh-keygen没有空格)
	ssh -T git@gitHub.com 验证ssh key


# 远程仓库

	git fetch <repository名称> 						从远程查看抓取数据，不合并
	git clone address			 					克隆远程仓库并且自动关联
	
	//如果是在本地init的git仓库，而远程仓库也存在，则使用下面两个步骤关联
	git remote add <origin> <git@github.com:Ztiany/studyGit.github>  关联远程仓库
	git pull origin master  拉即 一定要指定分支名
	
	git branch --set-upstream <branch-name> <origin>/<branch-name> 	建立本地分支和远程分支的关联
	
	git push -u orgin master 			推送到远程仓库，并建立关联 （远程仓库是空的）
	git push orgin master				远程仓库没有此分支则推送此分支，否则就是推送提交的内容
	git push orgin <branch-name> 									推送指定分支
	
	git pull orgin master				拉取文件并合并
	
	切换远程分支的时候先：git fetch  


	git checkout --track <origin>/<serverfix>					跟踪远程分支
	git push origin :<branch-name>								删除远程分支
	git checkout -b <branch-name> <origin>/<branch-name>		在本地创建和远程分支对应的分支 前提是远程仓库有这个分支
	git remote rm paul											删除远程仓库


# 分支

	git branch				查看分支
	git branch <name>  		新建分支
	git checkout <file> 		切换分支 	
	git checkout -b <name> 	新建并且切换
	git branch -d <name> 		删除
	git branch -D <name> 		强行删除未合并的分支

# 合并

	git merge <name> 							把指定分支合并到当前分支，Fast-forward信息，合并是“快进模式”，也就是直接把master指向dev的当前提交，
	git merge --no-ff -m "合并说明" <name> 		禁用快速合并,加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。


merge的说明：把两个分支最新的快照（C3 和 C4）以及二者最新的共同祖先（C2）进行三方合并，合并的结果是产生一个新的提交对象（C5）
![](https://git-scm.com/figures/18333fig0328-tn.png)




	git rebase master 		把在一个分支里提交的改变移到另一个分支里重放一遍。

							它的原理是回到两个分支最近的共同祖先，根据当前分支（也就是要进行衍合的分支 experiment）后续的历次提交对象（这里只有一个 C3），
							生成一系列文件补丁，然后以基底分支（也就是主干分支 master）最后一个提交对象（C4）为新的出发点，
							逐个应用之前准备好的补丁文件，最后会生成一个新的合并提交对象（C3'），
							从而改写 experiment 的提交历史，使它成为 master 分支的直接下游
	
							实际上是把解决分支补丁同最新主干代码之间冲突的责任，化转为由提交补丁的人来解决
	
							一旦分支中的提交对象发布到公共仓库，就千万不要对该分支进行衍合操作。

rebase:原理是回到两个分支最近的共同祖先，根据当前分支（也就是要进行衍合的分支 experiment）后续的历次提交对象（这里只有一个 C3），生成一系列文件补丁，然后以基底分支（也就是主干分支 master）最后一个提交对象（C4）为新的出发点，逐个应用之前准备好的补丁文件，最后会生成一个新的合并提交对象（C3'），从而改写 experiment 的提交历史，使它成为 master 分支的直接下游，
![](https://git-scm.com/figures/18333fig0329-tn.png)
衍合能产生一个更为整洁的提交历史。如果视察一个衍合过的分支的历史记录，看起来会更清楚：仿佛所有修改都是在一根线上先后进行的，尽管实际上它们原本是同时并行发生的。



# 工作区stash

	git stash 			储藏工作区
	git stash list 		查看储藏
	git stash apply 	恢复工作区
	git stash drop		删除储藏
	git stash pop		恢复并删除储藏


# 查看远程仓库

	git remote 		查看远程仓库信息
	git remote -v	详细信息
	git remote show <remoteName> 查看指定仓库信息
	git remote rename <oldname> <newname> <远程查看重命名,注意，对远程仓库的重命名，也会使对应的分支名称发生变化，原来的 pb/master 分支现在成了 paul/master。>


# TAG

	git tag 							查看tag
	git tag name 						打tag
	git tag name commitId 				根据id打tag
	git tag name -m "tag说明" commitId 	tag时，说明
	git push origin tag-name			推送tag到远程仓库
	git push origin --tags				推送本地所有tag
	git tag -d name 					删除tag
	git push origin :refs/tags/name 	删除远程tag


# Git子模块
>子模块就是你的一个Git项目需要应用其他Git项目，但是希望子模块嫩腿独立关联远程仓库，。

    git submodule add <git://github.com/chneukirchen/rack.git rack> 在主项目下使用此命令关联子模块，如果操作成功，会在根目录生成.gitmodules文件


    如果拉取了一个含有子模块的项目，需要执行以下命令
    首先拉取住主项目，命令略
    git submodule init 初次拉取必须使用此目录初始化子模块
    git submodule update 然后更新子模块，主模块更新不会自动同步子模块，每次需要同步子模块都需要子命令
    详情查看：https://git-scm.com/book/zh/v1/Git-%E5%B7%A5%E5%85%B7-%E5%AD%90%E6%A8%A1%E5%9D%97


# 快捷配置

	git config --global color.ui.true
	git config --global alias.st status 			 	配置别名
	git config --global alias.unstage 'reset HEAD'		配置别名  之后用git unstage file
	git config --global alias.last 'log -1'				配置别名  之后用git last
	
	git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"



# 文件匹配符号

	dir/ 表示目录
	dir/\*.class  表示dir下面的所有.class 结尾的文件
	
	使用实例：
	git rm log/\*.log 删除log目录下所有 ".log" 结尾的文件
	git rm \*~ 递归删除当前目录及其子目录下的所有 "~" 结尾的文件

# 技巧

	提示命令：
	git st 忘了命令，连续两次tag，会有提示


# 忽略文件

	在.gitignore中编辑

# 冲突


	CONFLICT 冲突
	<<<<<<< HEAD //表示当前分支内容
	Creating a new branch is quick & simple.
	=======
	Creating a new branch is quick AND simple.
	>>>>>>> feature1 //表示被合并分支的内容



# 分支策略

	首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
	干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本

git开发流程图如：![](http://www.liaoxuefeng.com/files/attachments/001384909239390d355eb07d9d64305b6322aaf4edac1e3000/0)

# gerrit

添加配置修改

```
scp -p -P 8091 weizhijian@gerrit.workec.com:hooks/commit-msg .git/hooks/

git config remote.origin.push refs/heads/*:refs/for/*
```