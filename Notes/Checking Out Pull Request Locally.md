---
category: note
type: tip
description: 將遠端 PR 分支拉下本地端查看
tags:
  - git
resource_1: https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests/checking-out-pull-requests-locally
---
如果你是一位需要幫其他工程師 code review 的主管或 lead，應該會有很多時機需要測試此 PR 新增的內容是否可以如預期執行，又或者你想要額外增加一些改動或調整，想要達到這個目的我們可以將此 PR 的遠端分支拉到本地端查看。

首先我們需要先拿到此 PR 的 ID，它會出現在 PR 標題的最後方 (不包含 `#`)
![[pull-request-id-number.webp]]

然後我們可以透過 `fetch` 加上 ID 將這個分支下載並複製到我們指定的分支上
```zsh
git fetch origin pull/<ID>/head:<BRANCH_NAME>
```

接著在切換到此分支即可查看跟操作
```zsh
git switch <BRANCH_NAME>
```

當我們操作完後便可以將這個分支推到遠端後開 PR 或者將其刪除
```zsh
git push origin <BRANCH_NAME>
```