# 在Git中撤销一切

[TOC]

## 写在前面

最近经常会从同事中听到这样的声音

> “这个文件不是我修改的，怎么忽略不掉？”
>
> ”不小心提交了一个配置，你们拉代码的时候小心一点”

项目组使用git也有半年多的时间了，面对这样的痛点,是时候更优雅地使用Git，让它帮我们做更多的事情。

## 场景

1. 撤销一个公共修改

   > 你刚刚使用git push 将本地修改推送到了github，这时你意识到在提交中有一个错误，你想撤销这次提交

   ```
   git revert SHA值
   ```

2. 修改最近一次的提交信息

   > 你只是在最后的提交信息中敲错了字，比如你敲了git commit -m "Fxies bug #42"，而在执行git push之前你已经意识到你应该敲"Fixes bug #42"

   ```
   git commit --amend -m 评论
   ```

3. 撤销本地更改

   > 当你的猫爬过键盘时，你正在编辑的文件恰好被保存了，你的编辑器也恰在此时崩溃了。此时你并没有提交过代码。你期望撤销这个文件中的所有修改——将这个文件回退到上次提交的状态。

   ```
   git checkout -- 文件名
   git checkout 文件名
   如果被stage了，则需要先reset head 操作将文件unstage后再checkout
   ```

4. 重置本地修改

   > 你已经在本地做了一些提交（还没push），但所有的东西都糟糕透了，你想撤销最近的三次提交——就像它们从没发生过一样。

   ```
   git reset SHA值或git reset --hard SHA值
   ```

5. 撤销本地后重做

   > 你已经提交了一些内容，并使用git reset –hard撤销了这些更改（见上面），突然意识到：你想还原这些修改！

   ```
   git reflog和git reset, 或者git checkout
   git reflog -n 显示前n条
   ```

6. 与分支有关的那些事

   > 你提交了一些变更，然后你意识到你正在master分支上，但你期望的是在feature分支上执行这些提交。

   ```
   git branch feature
   git reset --hard origin/master
   git checkout feature
   就是一个reset操作
   ```

7. 事半功倍处理分支

   > 你基于master新建了一个feature分支，但是master分支远远落后与origin/master。现在master分支与origin/master同步了，你期望此刻能在feature下立刻commit代码，并且不是在远远落后master的情况下。

   ```
   git checkout feature
   git rebase master
   与6的操作区别在于可以保留本地的提交记录
   ```

8. 批量撤销

   > 你开始朝一个既定目标开发功能，但是中途你感觉用另一个方法更好。你已经有十几个提交，但是你只想要其中的某几个，其他的都可以删除不要。

   ```
   git rebase -i
   Vim操作：
   i → Insert 模式，按 ESC 回到 Normal 模式.
   x → 删当前光标所在的一个字符。
   :wq → 存盘 + 退出 (:w 存盘, :q 退出)   （陈皓注：:w 后可以跟文件名）
   dd → 删除当前行，并把删除的行存到剪贴板里
   p → 粘贴剪贴板
   ```

9. 修复早先的提交

   > 之前的提交里落下了一个文件，如果先前的提交能有你留下的东西就好了。你还没有push，并且这个提交也不是最近的提交，因此你不能用commit –amend。 

   ```
   还是rebase的操作
   ```

10. 停止跟踪一个已被跟踪的文件

    > 你意外将application.log添加到仓库中，现在你每次运行程序，Git都提示application.log中有unstaged的提交。你在.gitignore中写上”*.log”，但仍旧没用——怎样告诉Git“撤销”跟踪这个文件的变化呢？

    ```
    git update-index --assume-unchanged name.file
    取消git update-index --no-assume-unchanged name.file; 
    当然还有一种做法stop tracking, 即git rm --cached name.file
    每个人同时进行stop tracking操作
    如果有例外,则进行git checkout检出删除前的文件,默认是被stage的,需要git reset移除stage
    ```


##  参考

[git操作](http://mobile.51cto.com/hot-481240.htm)

