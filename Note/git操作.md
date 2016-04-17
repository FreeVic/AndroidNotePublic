git 操作
-----
[TOC]
###1. 设置Git的user name和email：
$ git config --global user.name "vic"
$ git config --global user.email "vic@gmail.com"

###2. 生成SSH密钥过程：
1. 查看是否已经有了ssh密钥：cd ~/.ssh
如果没有密钥则不会有此文件夹，有则备份删除
2. 生存密钥：

		$ ssh-keygen -t rsa -C “haiyan.xu.vip@gmail.com”
		按3个回车，密码为空。
		Your identification has been saved in /home/tekkub/.ssh/id_rsa.
		Your public key has been saved in /home/tekkub/.ssh/id_rsa.pub.
		The key fingerprint is:
		………………
		最后得到了两个文件：id_rsa和id_rsa.pub

3. 添加密钥到ssh：ssh-add 文件名 需要之前输入密码。
4. 在github上添加ssh密钥，这要添加的是“id_rsa.pub”里面的公钥。
5. 测试：ssh git@github.com


###3. 开始使用github
1. 获取源码：
$ git clone git@github.com:billyanyteen/github-services.git
2. 这样你的机器上就有一个repo了。
3. git于svn所不同的是git是分布式的，没有服务器概念。所有的人的机器上都有一个repo，每次提交都是给自己机器的repo
仓库初始化：
git init
生成快照并存入项目索引：
git add
文件,还有git rm,git mv等等…
项目索引提交：
git commit
4. 协作编程：
将本地repo于远程的origin的repo合并，
推送本地更新到远程：
git push origin master
更新远程更新到本地：
git pull origin master
5. 补充：

    添加远端repo：
    $ git remote add upstream git://github.com/pjhyett/github-services.git
    重命名远端repo：
    $ git://github.com/pjhyett/github-services.git为“upstream”

###4. 其他
####4.1 忽略
untrack的文件添加gitignore文件，对于track的文件使用git update-index --assume-unchanged name.file，这样本地修改就再也不会出现在未提交的更改中了，取消这种设置使用git update-index --no-assume-unchanged name.file

####4.2 查找提交
2. 不带任何参数：git log 会列出所有提交，最近的排在最上面，按Q退出查看
3. 显示前n条：git log --n
4. 显示简要的增改行数统计：git log --stat
5. 同上，显示得更全：git log -p
6. 一行显示：只显示hash值和提交说明，git log --pretty=oneline，等同于git log –oneine
7. 显示简单图形：git log –graph
8. 控制显示的记录格式：git log --pretty=format:””，各占位符意义

	    a)%H  提交对象（commit）的完整哈希字串
	    b)%h  提交对象的简短哈希字串
	    c)%T  树对象（tree）的完整哈希字串
	    d)%t  树对象的简短哈希字串
	    e)%P  父对象（parent）的完整哈希字串
	    f)%p  父对象的简短哈希字串
	    g)%an 作者（author）的名字
	    h)%ae 作者的电子邮件地址
	    i)%ad 作者修订日期（可以用 -date= 选项定制格式）
	    j)%ar 作者修订日期，按多久以前的方式显示
	    k)%cn 提交者(committer)的名字
	    l)%ce 提交者的电子邮件地址
	    m)%cd 提交日期
	    n)%cr 提交日期，按多久以前的方式显示
	    o)%s  提交说明

9. 指定路径：git log --pretty=oneline filePath
10. 指定日期、关键字、作者、提交者

	    a)日期：git log --since=10.days，同样，类似的参数还有after、until、before
	    b)关键字：git log --grep=keyword
	    c)作者：git log author=author
		d)提交者：git log --committer=committer
