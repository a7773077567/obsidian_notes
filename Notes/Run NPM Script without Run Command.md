---
category: note
type: problem
tags:
  - node
  - js
  - npm
description: 執行 NPM 腳本不需要 Run 指令
---
使用 NPM 執行 Script 的時候，有時候需要加 run 指令，有時候卻不需要。這很有可能是因爲執行了 npm start 這個 script，因為 start 這個腳本是特別的 case，而其他的 script 需要加 run 否則會報錯。