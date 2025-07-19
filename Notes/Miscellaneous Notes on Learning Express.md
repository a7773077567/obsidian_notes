---
category: note
type: info
tags:
  - node
  - express
description: 學習 express 的一些雜記
---
- app.use() 收的 path 代表的並非絕對路徑，而是開頭是否符合，例如 `app.use('/')` 同時可以符合 / 跟 /add-product 這兩個路徑。
- request handler 在 express 可以用 app.use 將其註冊為 middleware，每個 middleware 可以使用 next 來前往下一個 middleware。
- middleware 擺放的順序很重要，例如 `app.use('/add-product')` 應該在 `app.use('/')` 之前 response，這樣 /add-product 才不會先被 `app.use('/')` 抓到。
- 如果 middleware 沒有指定 path，那麼將會由註冊的順序由上而下往下依順序決定是否執行，middlerware 中如果有呼叫 next()，將會前往下一個 middleware。
- 當瀏覽器收到的 response 是 html 時，瀏覽器會再往伺服器發送 /favicon.ico 的請求。
- 我們可以將 html 檔存放在 views 資料夾下，然後利用 res.sendFile 選取對應的 html 檔發送。但需要注意路徑的填寫方式，如果直接指名 / 會是這台電腦的 root 而非此專案資料夾的 root，但如果使用 ../ 或 ./ 會得到必須使用絕對路徑的錯誤回報；我們可以使用 node core module 的 path module 加上 node 的全域變數 `__dirname` 來組合正確的絕對路徑，例如
	- `path.join(__dirname, '../', 'views', 'shop.html')`