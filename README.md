# Git_Note
note some git usage 

## Git: The three states that your files can reside in

1. Committed: means that the data is safely stored in your local database.

2. Modified: means that you have changed the file but have not committed it to your database yet.

3. Staged: means that you have marked a modified file in its current version to go into your next commit snapshot.
```git file states
working tree <-> staging area <-> .git directory
      |                 |               |
      | <---_ checkout the project ---- |
      | -- stage fix -> |               |
      |                 | -- commit --> |
```
* .git directory: git 裡面最重要的部分,儲存project(repository)所有的metadata and object database
*  working tree: 這個project的某一個版本裡的檔案,可以讓使用者來修改
* staging area: 也叫index,是一個在 .git裡面的檔案, 儲存使用者要用來下一個commit的資料
```
基本工作流程:
1.在working tree中修改檔案
2.把想要加入下一個commit的檔案add進staging area
3.提交commit,然後Git會把staging area的資料存進資料庫
```

常用指令 help, config, init, clone, 

# help

git help \<command>

man git-\<command>

git \<command> -h

# Setup and Config(會更改或讀取 .gitconfig)

## git config

git config --list

git config user.name

git config --global user.name "username"

git config user.email

git config --global user.email "email@eee.com"

git config --global core.editor /path/to/your/editor

ex. git config --global core.editor /urs/bin/vim

git config --global alias.xx command (add alias

ex. git config --global alias.st status

# Getting and Creating Projects

## git init

## git clone

# Basic Snapshotting

## git status (用來確認目前檔案的狀態)
usage: git status

The lifecycle of the status of your files:
```
Untrack(unstaged) <-> Unmodified <-> modified(unstaged) <-> Staged
       |                  |                  |               |
       |  --------------------- add file ------------------> |
       |                  |- edit the file ->|               |
       |<-remove the file-|                  |               |
       |                  | <----------------- commit -------|  
```
在上面介紹到的基本工作流程中, 首先在此版本中的檔案都尚未被修改前,
所有檔案的狀態都是 "Unmodified", 然後進入基本工作流程1,
也就是修改檔案(或是增加檔案), 這些修改的檔案狀態會變成"modified(unstaged)",
而增加的檔案狀態會是"Untrack", 這些變動是造成不同版本的原因, 然後進入基本工作流程2,
把想要加入下一個版本的檔案add進去staging area, 
而被add進去staging area的檔案狀態就是"Staged",
最後進入基本工作流程3,提交commit! 這時會把在Staged的檔案存進資料庫,
也就成為了新的commit(版本),而在這時候這些檔案的狀態就會回到"Unmodified"

## git add (把unstaged的檔案加進staging area)
usage: git add <filename>

git add . (把全部unstaged的檔案都add成staged)

git add hello.py 

git add -p filename(patch 一段一段檢視要add的code, 可以部分add)

## git diff

git diff (查看修改部分和上一個commit版本的不同)

git diff A B (從 A branch commit 到 B branch commit 的不同)

git diff --cached (查看在add內(staged)但是未commit的和上一個commit版本的不同)

git diff HEAD (一起查看上面兩者)

## git difftool

## git commit

git commit

git commit -m '你的commit紀錄'

git commit --amend 

git commit --amend(合併到上一個commit) --no-edit(不編輯)

git commit -am "create branch dev" ("-am":直接add所有改變,並且直接commit)

## git reset

git reset filename (把此次add到staged狀態的檔案,回到modifily)

git reset --hard HEAD (不管有沒有add,到上一個commit的版本,若無修改或add則停在當前版本)
		 HEAD^(回到上上個commit,git log --oneline與git reflog比較)
		 HEAD^^(上上上個版本, or HEAD~2)
		 
git reset --hard (給定版本id號碼 or id對應HEAD@{X},則直接回到此id版本)

# Branching and Merging

## git branch

git branch (查看當前分支,*為當前HEAD所在分支)

git branch dev(建立分支dev)

git branch -d dev(刪除分支dev,要在別的分支才可以)
           -D    (強制刪除)

## git checkout (指針可以讓任意檔案或版本回到某個時刻 移動HEAD)

git checkout id-- filename (單一文件filename返回此id時版本)

git checkout file 把unstaged(未add)狀態的file回到原本commit 為修改狀態

git checkout -b xxx

## git merge

git merge dev(在HEAD所指的分支把dev合併過來,不會有commit訊息並且log中不會有圖案)

git merge --no-ff -m "keep merge info" dev (--no--ff -m "info" : 保留commit訊息)

merge分支衝突(如果新分支修改檔案,原本分支檔案也修改,然後在原本分支要合併會有分支衝突,
              此時打開合併衝突的檔案,會有衝突標示,手動改好之後在add跟commit即可)

## git log

git log (修改紀錄,目前版本與以前)

git log --oneline

git log --oneline --graph(圖示)

git reflog(查看全部修改版本)

git log <path>|<commit>
        -n : limit line
        -stat : view file change
        -paych : view lines change
        -S or --grep : find commmit have grep text

## git stash

git stash (把目前版本(沒有staged的)放到暫存區,然後你就可以去做其他事情溜!)

git stash pop (返回)
          list
          drop

## git tag

# Sharing and Updating Projects

## git fetch

## git pull

## git push

## git remote

git remote add remotename url(加遠端repo地址,clone下來的那包,remote預設叫origin)

# Inspection and Comparison

## git show

# Patching

## git rebase (會更動歷史,所以只能在自己的分支使用,從某個點的commit條拔起來,插到某個commit上的概念)

git rebase dev(在HEAD所指的分支(假設是master),把dev分支合併過來)

分支衝突:修改衝突檔案後使用git branch檢查所在分支,
         會發現所在分支停在(no branch,rebasing master),
         而不是master或dev,此時需要add檔案
         
git add (衝突檔案)

git rebase --continue(完成合併修改,rebase會把爬起來的commit條,一個一個重新commit到你要插上去的commit上)

git rebase onto

git rebase -i (互動模式)
