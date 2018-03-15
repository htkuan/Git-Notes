# Git_Note
note some git usage 

介紹常用git指令與概念
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

git 分為四種物件(commit tree blob tag) 會存在 .git/object/ 下
資料夾回 hash值的前兩位, 物件關聯為 
某一個commit物件(或是版本)->關聯一些tree物件(資料夾概念)->
而每一個tree物件都會紀錄他底下又關聯了哪些tree物件或是blob物件->最後都會關聯到一個blob物件

blob物件就是版本的文擋(只根據文檔內容運算過的檔案,所以同樣內容必定會指向同一個blob物件)
tree就表示資料夾與階層,所以要是版本內有兩個地方有同樣的文檔->經過tree的連接->最後會指到同一個blob物件

snapshot概念就是 commit經過tree連結到所有的blob檔,就代表了此版本所有的文檔紀錄,並不是只記錄commit中的改變

tag物件就純粹是 commit指到tag, 代表某一個tag

git cat-file -p obj_id  # 可以查看物件內容

.git/refs/ 下存參照名稱(會對應到一個hash絕對名稱)
本地分支：.git/refs/heads/
遠端分支：.git/refs/remotes/
標　　籤：.git/refs/tags/

git update-ref
git symbolic-ref
git show-ref

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
usage: git init

This creates a new subdirectory named .git that contains all of your necessary repository files 

— a Git repository skeleton.

## git clone
usage: git clone \<url> <repo_name>

ex.

```
$ git clone https://github.com/username/repo_name  //直接複製repo下來
$ git clone https://github.com/username/repo_name local_repo_name //複製repo並改名成指定名稱
```

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

## .gitignore (Ignoring Files)
如果有不想被Git記錄進版本控制的檔案,可以利用編輯 .gitignore放在同 .git資料夾階層。

ex.

```
# ignore all .a files
*.a

# but do track lib.a, even though you're ignoring .a files above
!lib.a

# only ignore the TODO file in the current directory, not subdir/TODO
/TODO

# ignore all files in the build/ directory
build/

# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt

# ignore all .pdf files in the doc/ directory and any of its subdirectories
doc/**/*.pdf
```
## git add (把unstaged的檔案加進staging area)
usage: git add <filename>

ex.
```
$ git add .  //把全部unstaged的檔案都add成staged
$ git add hello.py  //把hello.py加成staged
$ git add -p filename  //patch 一段一段檢視要add的code, 可以部分add
```

## git diff

git diff (查看修改部分和上一個commit版本的不同)

git diff A B (從 A branch commit 到 B branch commit 的不同)

git diff --staged

git diff --cached (查看在add內(staged)但是未commit的和上一個commit版本的不同)

git diff HEAD (一起查看上面兩者)

git diff                 => 工作目錄 vs 索引

git diff HEAD            => 工作目錄 vs HEAD

git diff --cached HEAD   => 索引     vs HEAD

git diff --cached        => 索引     vs HEAD

git diff HEAD^ HEAD	     => HEAD^   vs HEAD

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

git reset --hard ORIG_HEAD (還原到reset前的狀態)

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
              
ps. 一般來說merge如果有衝突,解完衝突需要commit,所以會再merge後多出新的commit,
    若沒有衝突則是 fast-forward 則主branch的最後一個commit會重疊merge進來的branch的
    最後一個commit, 下 --no--ff 是讓merge不要是fast-forward,所以會在兩個branch merge時,
    多出一個 merge commit!

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
          
git stash 會將所有已列入追蹤(tracked)的檔案建立暫存版

git stash -u　會包括所有已追蹤或未追蹤的檔案，全部都建立成暫存版

git stash save -u <message> 指令，就可自訂暫存版的註解：

## git tag

列出所有標籤：git tag

建立輕量標籤：git tag [tagname]

刪除輕量標籤：git tag [tagname] -d

建立標示標籤：git tag [tagname] -a

輕量標籤不會建立 tag物件

可以在加上 -m 'message' 給標示標籤訊息

# Sharing and Updating Projects

## git fetch
usage: git fetch \<remote_name> \<branch>

ex. git fetch upstream develop

## git pull
usage: git pull \<remote_name> \<branch>
```
$ git pull <remote_name> <branch>
same as
$ git fetch <remote_name> <branch>
            +
$ git merge <remote_name> <branch>
```
ex. git pull upstream develop

## git push
usage: git push \<remote_name> \<branch>

ex. git push origin master

## git remote

git remote add remotename url(加遠端repo地址,clone下來的那包,remote預設叫origin)

# Inspection and Comparison

## git show

# Patching

## git rebase (會更動歷史,所以只能在自己的分支使用,從某個點的commit條拔起來,插到某個commit上的概念)

git rebase dev(在HEAD所指的分支(假設是master),把dev分支合併過來, rebase會尋找兩branch的交叉點,
               然後把交叉點到HEAD的commit條拔起來,一個一個commit到dev最新的commit上,
               成為master的最新commit條, 好處就是當master要merge進去dev時會是 fast-forward)

分支衝突:修改衝突檔案後使用git branch檢查所在分支,
         會發現所在分支停在(no branch,rebasing master),
         而不是master或dev,此時需要add檔案
         
git add (衝突檔案)

git rebase --continue(完成合併修改,rebase會把爬起來的commit條,一個一個重新commit到你要插上去的commit上)

git rebase onto

git rebase -i HEAD~~~~~~(互動模式, 幾個~ 就回朔幾個commit, 並且可以任意改動這些commit)

