---
title: git使用中遇到的错误
date: 2017-03-14 18:41:18
tags: [git, 笔记]
---

用以记录在使用git时遇到的错误方便自查~

---

使用`git push`或`git push origin master`（向远程仓库提交）时遇到错误：
	```
    To https://github.com/XXXX/XXXX.git
     ! [rejected]        master -> master (fetch first)
    error: failed to push some refs to 'https://github.com/XXXX/XXXX.git'
    hint: Updates were rejected because the remote contains work that you do
    hint: not have locally. This is usually caused by another repository pushing
    hint: to the same ref. You may want to first integrate the remote changes
    hint: (e.g., 'git pull ...') before pushing again.
    hint: See the 'Note about fast-forwards' in 'git push --help' for details.
    ```
原因：远程仓库中的master分支（假设仅有一个master分支）有新的提交而本地仓库没有更新同步到这个提交，所以本地仓库向远程仓库推送提交的时候会报出这个错误。解决的方法是先把远程仓库的提交抓取下来再提交。
    ```
    $ git pull
    $ git push
    ```
注意：`git pull`会提示"Please enter a commit message to explain why this merge is necessary, especially if it merges an updated upstream into a topic branch。"，意思是输入可以描述此次合并分支的提交信息。可以选择写也可以不写，然后按下`Esc`键输入`:wq`（强制性写入文件并退出），按下`Enter`就可以了。


