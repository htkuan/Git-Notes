# Git Advanced Usage

# Basic Snapshotting

## git difftool

## git rm

## git mv

## git clean

# Branching and Merging

## git mergetool

# Sharing and Updating Projects

## git archive

## git submodule

# Inspection and Comparison

## git shortlog

## git describe

# Debugging

## git bisect

## git blame

## git grep

# Patching

## git cherry-pick

git cherry-pick <commit hash>

把此選定的commit改動 加到現在的HEAD上

git cherry-pick -x <commit hash>

加上 -x 會在commit message的detail中加上 (cherry picked from commit <commit hash>)

## git revert

git revert <commit hash>

會新增一個 revert commit 並且把指定 commit 上的修改復原

# Email

## git apply

## git am

## git format-patch

## git imap-send

## git send-email

## git request-pull

# External Systems

## git svn

## git fast-import

# Administration

## git gc

## git fsck

## git reflog

## git filter-branch

