---
category: note
type: problem
tags:
  - favicon
  - request
  - node
  - api
description: 瀏覽器自動對 root path 發出的請求
---
在學習 Nodejs 的時候，總是發現會額外收到一個對 root path 的 GET 請求，這是因為瀏覽器在得到一份 HTML 後會自動對伺服器額外發出一個 /favicon.icon 的請求，這個行為可以參考 [[Favicon Request Behavior]] 。

但我的 middleware 並沒有針對 /favicon.ico 來進行處理，因此總是會被 / 這個 middleware 給抓到，所以才會誤以為瀏覽器一直對 root path 發出請求。