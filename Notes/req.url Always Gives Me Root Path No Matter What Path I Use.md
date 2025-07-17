---
category: note
type: problem
tags:
  - node
  - express
  - router
  - middleware
description: 在 express 中，不管那個路由的 middleware 其 req.url 都是 root path
resource_1: https://stackoverflow.com/questions/41496569/req-url-always-gives-me-no-matter-what-path-i-use
resource_2: https://expressjs.com/en/5x/api.html#req.originalUrl
---
在 express middleware 中的 req.url 並非是原生的 express 屬性，它是繼承 Node.js 的 http module 而來的，非常值得注意的是 app.use 的 mounting feature 會將 req.url 覆蓋(rewrite)為 root path (`/`)？

應該要使用 req.originalUrl 來取得最原始的 URL。