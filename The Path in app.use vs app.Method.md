---
category: note
type: problem
tags:
  - node
  - express
  - router
description: Route path 在 app.use 跟 app.Method 的 match 模式不同
---
透過 app.Method 或 router.Method 來寫 middleware 時，其 path 會變成 exact match，也就是必須完全符合的 route 才會被這個 middleware 執行，因此 middleware 擺放的順序就不太重要。

這個行為跟透過 app.use 寫的 middleware 不同，因為此時的 path 是只要符合開頭便會被抓到，因此全部的 route 都會被 / 這個 route 給抓到，因此 middleware 擺放的順序就會非常重要。