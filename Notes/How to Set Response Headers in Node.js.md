---
category: note
type: tip
tags:
  - node
  - js
  - http
  - header
description: 如何在 Node.js 寫入回應的標頭（header）
---
當 server 收到一個 request 後，可以透過 requestListener 的第二個 res 參數來寫入 head 並回應給使用者，會使用到以下兩個方法：
- res.setHeader
- res.writeHead

res.setHeader 一次可以寫入一個 header，可以多次呼叫寫入多個不同的標頭，此時這些標頭只是在內部暫存區被設定，還不會被傳送給使用者。

而 res.writeHead 則可以同時寫入多個 header，但必須同時指定狀態碼，且一旦被呼叫就會將標頭強制傳送給使用者，也就是必須在 res.end 前被呼叫。

因此 res.writeHead 只能呼叫一次，而 res.setHead  在 res.writeHead 被呼叫後就不能再呼叫。