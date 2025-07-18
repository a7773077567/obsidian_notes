---
category: note
type: info
tags:
  - browser
  - favicon
  - node
  - api
description: favicon.ico 的請求行為
---
favicon 可以用來作為個網站的一個識別標識，它通常會出現在瀏覽器的標籤上、書籤列、歷史列表或添加到桌面時。

當瀏覽器第一次載入此網站時便會嘗試發送 GET /favicon.icon 的請求來獲取 favicon，並將其快取起來，如果我們沒有在 HTML 的 `<head>` 透過 `<link rel="icon" href="path/to/favicon.ico">` 來指定 favicon 的路徑，或者它在這個路徑下沒找到 favicon，它便會退回到 root 去尋找。

當快取過期或瀏覽器認為需要更新 favicon 時便會再度發送這個請求。

