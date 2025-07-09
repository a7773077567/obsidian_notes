---
category: note
type: info
tags:
  - node
  - js
description: 學習 Node.js 的一些雜記
---
## Facts

- Nodejs 跟 Browser 都是使用 V8 來將程式碼編譯成 machine code，而 V8 是使用 C++ 寫成的
- Nodejs 本身就可以作為 server，它自己就可以執行 javascript，這點跟 PHP 不一樣，PHP 則是程式碼是交由 apache 或 nginx 來決定是否執行
- 只要 Node.js 還有 listener 還在監聽那麼程式就不會結束，但我們可以透過 `process.exit()` 強制結束

## Functions

- 可以使用 writeFileSync 寫入檔案
-  我們可以從 core module 中引入 http class 來創造一個 server 並運行 - `http.createServer`
	- 接收 options 跟 requestListener，2 個竟然都是可選的，可能是 function overload？
	- 這裡的 requestListener 看起來是沒有 route 的概念，所有的 route 都會觸發這個 listener
	- requestListener 似乎會直接被 attach 到 request event，因此當有用戶發送 request 時便會直接觸發我們丟進 createServer 的 callback
	- createServer 回傳的是 http.Server class，我們可以使用 server.listen 啟動一個 HTTP server
	- listener 會收到兩個參數，分別是 req 跟 res
		-  可以透過 req 拿到很多資訊，包括 url、method 及 headers 等等
		- 我們可以透過 res 回應使用者，例如透過 res.setHeader 寫入 Content-Type，或者使用 res.write 寫入 html，最後使用 res.end 來將

## Concepts

- 當使用者從瀏覽器輸入網址後便會發生一系列事情，首 DNS server 會去解析這段網址找出其對應的 IP 以便連結到正確位址的 server，此時 server 便會做出反應，可能為以下：
	- 回傳一份 HTML
	- 跟 Database 互動
	- 進行身份驗證
	- 進行輸入驗證
	- 執行商業邏輯
- Nodejs 在 Web development  的角色
	- run server - create server & listen to incoming requests
	- business logic - handle request, validate input, connect to database
	- responses - return responses, rendered HTML, JSON...
- The REPL
	- Read - Read user input
	- Eval - Evaluate user input
	- Print Print output (result)
	- Loop - Wait for new input