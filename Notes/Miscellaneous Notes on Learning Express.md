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
	- `path.join(__dirname, '..', 'views', 'shop.html')`
	- path.join 可以將 path segment 組合成一個完整的 path string
	- path.join 會自動加上 /，所以每個 segment 可以忽略填寫 /
	- path.join 有一個很重要的功能，它可以偵測目前的作業系統，組合成符合系統的正確路徑
		- linux 使用 / (slash) 作為路徑的 segment 分隔
		- windows 使用 \ (back slash) 作為路徑的 segment 分隔
- 可以創造一個 helper module 來提供我們正確的專案 root 絕對路徑，例如以下
	- `path.dirname(require.main.filename)`
	- `path.dirname(process.mainModule.filename)` (deprecated)
		- path.dirname 可以將一個檔案路徑去除檔案的 segment，得到其資料夾的路徑
		- require.main.filename 跟 process.mainModule.filename 都可以取得專案資料夾的 root，也就是我們用 node 指令執行的那隻檔案，通常會是 app.js、index.js 或是 server.js 等等...
- 當我們連線到一個 server 時，理論上我們可以根據正確路徑取得存在檔案系統的檔案，但實際上 server 會設置權限防止使用者隨意存取各個檔案，例如 express 就會根據使用者的路徑找到對應的 middleware 並給予相對應的檔案、資料或行為，沒有 match 到的一律拒絕存取。但有時候有些檔案我們會想要讓其可以公開存取，我們會將其放在一個慣用名稱為 public 的資料夾，並透過 express 將其設定為 static 資料夾，此路徑底下的請求便不需要經過 middleware 即可直接存取。
	- `app.use(express.static(path.join(__dirname, 'public')))`