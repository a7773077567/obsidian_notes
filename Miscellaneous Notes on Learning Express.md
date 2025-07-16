---
category: note
type: info
tags:
  - node
  - express
description: 學習 express 的一些雜記
---
- app.use() 收的 path 代表的並非絕對路徑，而是開頭是否符合，例如 `app.use('/')` 同時可以符合 / 跟 /add-product 這兩個路徑。
- request handler 又稱為 middleware，每個 middleware 可以使用 next 來前往下一個 middleware。
- middleware 擺放的順序很重要，例如 `app.use('/add-product')` 應該在 `app.use('/')` 之前 response，這樣 /add-product 才不會先被 `app.use('/')` 抓到。
- 