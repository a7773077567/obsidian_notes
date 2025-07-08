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
- 

## Functions

- 可以使用 writeFileSync 寫入檔案

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
- 