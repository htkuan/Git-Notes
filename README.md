# Git_Note
note some git usage 

git status
git init

## Untrack---Unmodified---modified(unstaged)---Staged

### config setting(會更改或讀取 .gitconfig)

git config user.name

git config --global user.name "username"

git config user.email

git config --global user.email "email@eee.com"

git config --global alias.xx command (add alias

ex. git config --global alias.st status

### add and commit

git add filename(ro . = 加入所有檔案)

git add -p filename(patch 一段一段檢視要add的code, 可以部分add)

git commit

git commit -m '你的commit紀錄'

git commit --amend 

### log

git log (修改紀錄,目前版本與以前)

git log --oneline

git log --oneline --graph(圖示)

git reflog(查看全部修改版本)

git log <path>|<commit>
        -n : limit line
        -stat : view file change
        -paych : view lines change
        -S or --grep : find commmit have grep text
  
### diff

git diff (查看修改部分和上一個commit版本的不同)

git diff A B (從 A branch commit 到 B branch commit 的不同)

git diff --cached (查看在add內(staged)但是未commit的和上一個commit版本的不同)

git diff HEAD (一起查看上面兩者)

### reset

git commit --amend(合併到上一個commit) --no-edit(不編輯)

ex.
commit後發現忘記有東西也想加進去這個commit
git add (補上修改的檔案)
git commit --amend --no-edit(此次commit將不會是一個新的commit,而是合併到上一個commit)

git reset filename (把此次add到staged狀態的檔案,回到modifily)
git reset --hard HEAD (不管有沒有add,到上一個commit的版本,若無修改或add則停在當前版本)
		 HEAD^(回到上上個commit,git log --oneline與git reflog比較)
		 HEAD^^(上上上個版本, or HEAD~2)
git reset --hard (給定版本id號碼 or id對應HEAD@{X},則直接回到此id版本)

### checkout(指針可以讓任意檔案或版本回到某個時刻 移動HEAD)

git checkout id-- filename (單一文件filename返回此id時版本)

git checkout file 把unstaged(未add)狀態的file回到原本commit 為修改狀態

### branch
git branch (查看當前分支,*為當前HEAD所在分支)
git branch dev(建立分支dev)
git branch -d dev(刪除分支dev,要在別的分支才可以)
           -D    (強制刪除)
git checkout dev(切換到分支dev)
git checkout -b dev (創建分支dev, 而且直接切換到分之dev)
git commit -am "create branch dev" ("-am":直接add所有改變,並且直接commit)

### merge

git merge dev(在HEAD所指的分支把dev合併過來,不會有commit訊息並且log中不會有圖案)

git merge --no-ff -m "keep merge info" dev (--no--ff -m "info" : 保留commit訊息)

merge分支衝突(如果新分支修改檔案,原本分支檔案也修改,然後在原本分支要合併會有分支衝突,
              此時打開合併衝突的檔案,會有衝突標示,手動改好之後在add跟commit即可)
              
### rebase(會更動歷史,所以只能在自己的分支使用,從某個點的commit條拔起來,插到某個commit上的概念)
git rebase dev(在HEAD所指的分支(假設是master),把dev分支合併過來)
分支衝突:修改衝突檔案後使用git branch檢查所在分支,
         會發現所在分支停在(no branch,rebasing master),
         而不是master或dev,此時需要add檔案
git add (衝突檔案)
git rebase --continue(完成合併修改,rebase會把爬起來的commit條,一個一個重新commit到你要插上去的commit上)
git rebase onto
git rebase -i (互動模式)

### stash
git stash (把目前版本(沒有staged的)放到暫存區,然後你就可以去做其他事情溜!)
git stash pop (返回)
          list
          drop
          
### remote
git remote
git remote add remotename url(加遠端repo地址,clone下來的那包,remote預設叫origin)
git push
git push origin master(推送本地端master去origin,也可以推送本地端其他分支)
git clone https://github.......(複製專案)
git pull(從遠端更新專案到本機)
