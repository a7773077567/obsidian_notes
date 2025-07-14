---
category: note
type: problem
tags:
  - vscode
  - debug
description: VSCode debug 模式中的 breakpoint 紅點消失
---
在 VSCode 中的 debug 模式可以直接點擊 line number 標記 breakpoint 的地方消失了，這是因為 Editor: Glyph Margin 被設定為 false 導致其消失。