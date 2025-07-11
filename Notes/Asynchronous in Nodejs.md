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
resource_1: https://nodejs.org/en/learn/asynchronous-work/event-loop-timers-and-nexttick
---
Nodejs 跟 Web Browser 的 Event Loop 其核心概念差不多，但在執行階段的設計不太一樣，Nodejs 有很明確的執行階段，且大部分的 event handler 是直接交給 event loop 掌控，但 file system 的 handler 是交給 Worker Poll 處理，也就是 Heavy Lifting 的工作會讓 worker 去開啟不同的 Thread 來處理，最後再通知 Event Loop 來處理其 handler。

![[SCR-20250710-obdk.png]]

Nodejs 的 Event Loop 有以下 Phases：
1. Timers
2. Pending Callbacks
3. Poll
4. Check
5. Close Callbacks
6. 是否關閉程式

可以將 Event Loop 想像成一個任務管理員，他的工作任務就是將任務分派給各個不同的人處理，而 Stack 便是那個唯一的執行者，他一次只能執行一個任務，而做什麼任務由 Event Loop 決定。

首先 Event Loop 會去 Timers 查看有沒有到期的計時器(setTimeout, setInterval)，如果有就執行這些到期的 callback。

接下來會進入 Pending Callbacks，這邊 Event Loop 會去檢查是否有因為前一個循環留下來的 pending callback，如果有就執行，例如作業系統級別的錯誤回報，這通常有 Nodejs 內部處理，使用者不太會接觸到。

然後就進到最關鍵的 Poll(輪詢) 階段，此時 Node 會馬上先去查看是否有新的 I/O 完成，並執行對應的 callback，接下來邊會查看 setImmediate 是否有 callback 需要執行，如果有，Poll 便會立刻結束並進入 Check 階段；如果沒有便會檢查是否有新的 I/O 產生決定是否要執行或者推遲這個 callback 到  Pending Callbacks，在 Poll 等待的過程中，如果有新的 Timer 到期，便會回到 Timer 階段，再往下走。

> [!NOTE] 何為 I/O 事件？
> 新的連線 net.createServer()
> 檔案讀取 fs.readFile()
> 網路請求 http.request()

到了 Check 階段，Node 會執行 setImmediate 的 callback。

最後的階段便是  Close Callbacks，此時會執行所有 close event 的 callbacks。

但 Event Loop 還存在著一個 Microtask Queue，以下的 callback 都會被放進這個 queue：
- process.nextTick()
- Promise.then()
- Promise.catch()
- Promise.finally()

Microtask 並不是一個獨立的階段，它會在進入下個階段前連續處理掉整個佇列的任務，而 process.nextTick 的優先級又是最高的。如果在處理 nextTick 跟 Promise 的 callback 時又有新的 nextTick 及 Promise callback，它們一樣會馬上被放到 Microtask queue 內並被清空，只有當整個佇列被清空時才會進入下一個階段。

最後當程式找不到任何 callback reference 時（refs === 0），便會終止程式的運行。


![[SCR-20250710-ocbt.png]]
![[SCR-20250711-tqil.png]]