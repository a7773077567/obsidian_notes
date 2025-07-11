---
category: note
type: info
tags:
  - node
  - async
  - js
  - event-loop
  - worker
description: Nodejs 的非同步是如何達成的
---
Nodejs 跟 Web Browser 的 Event Loop 其核心概念差不多，但在執行階段的設計不太一樣，Nodejs 有很明確的執行階段，且大部分的 event handler 是直接交給 event loop 掌控，但 file system 的 handler 是交給 Worker Poll 處理，也就是 Heavy Lifting 的工作會讓 worker 去開啟不同的 Thread 來處理，最後再通知 Event Loop 來處理其 handler。

![[SCR-20250710-obdk.png]]

Nodejs 的 Event Loop 有以下 Phases：
1. Tiemrs
2. Pending Callbacks
3. Poll
4. Check
5. Close Callbacks
6. 是否關閉程式

可以將

![[SCR-20250710-ocbt.png]]