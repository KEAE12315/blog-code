---
title: "Git常用"
date: 2022-08-05T16:50:39+08:00
draft: false
tags:
- Git
- tips
---

```
git checkout <branch>

git checkout master
git checkout -b cleaned_up_branch
git merge --squash private_feature_branch
git reset
```

将本地分支推送到远程分支

```
git push -u <remote> <local_branch>:<remote_branch>
```

在 git push 命令中，-u 参数是用来将本地分支与远程分支关联起来的。使用 -u 参数后，git 会将本地分支推送到远程分支，并在本地分支和远程分支之间建立一个跟踪关系，这样在以后的 push 和 pull 操作中就可以直接使用 git push 和 git pull 命令，而不需要再指定远程分支名和本地分支名。

## 参考

- [理解Git的工作流程](https://kb.cnblogs.com/page/152176/)