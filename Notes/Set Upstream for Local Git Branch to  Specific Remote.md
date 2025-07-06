---
category: note
type: tip
tags:
  - git
  - branch
description: 綁定本地分支至遠端
---
當我們需要將本地分支備份到遠端或者發 PR 的時候， 應該要將本地的分支跟遠端的分支做綁定，這樣在推送到遠端時就不需要每次都指定要推到哪個遠端分支。

使用 `git branch --set-upstream-to` 設定本地分支的上游
```zsh
git branch --set-upstream-to=<remoteName>/<remoteBranchName> <localBranchName>
```

```zsh
git branch --set-upstream-to=origin/feature-A my-feature-A
```

> [!TIP] 省略 localBranchName
> 如果已經在在當前分支上，可以省略 `<localBranchName>`


如果是第一次將本地分支推送到遠端，並希望同時建立追蹤關係，可以使用 `git push -u` 或者 `git push --set-upstream`
```zsh
git push -u <remoteName> <localBranchName>:<remoteBranchName> 
```

```zsh
git push -u origin my-feature-A:feature-A
```

如果本地分支名稱和遠端分支名稱相同，可以簡寫成以下
```zsh
git push -u <remoteName> <localBranchName>
```

```zsh
git push -u origin my-feature-A
```


