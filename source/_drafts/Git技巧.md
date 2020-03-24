---
title: Git技巧
tags:
---
## gitignore
如果你有文件(夹)已经check in,但你想ignore它。**如果你是之后增加的规则，Git不会ignore这个文件**.这时需要先untrack.
```
git rm --cached -r .
git add .
```