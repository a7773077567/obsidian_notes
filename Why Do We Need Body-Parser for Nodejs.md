---
category: note
type: info
tags:
  - express
  - node
  - parser
description: 為什麼我們需要 body-parser
resource_1: https://ithelp.ithome.com.tw/m/articles/10241083
---
在原生的 Nodejs 中接收資料其實是一件很麻煩的事，我們必須監聽 req.on('data') 跟 req.on('end') 事件，將每個 buffer chunk 轉換成其他可用的格式，就算我們只是傳輸 application/x-www-form-urlencoded，瀏覽器也會將其轉換成  buffer chunk。

我們可以使用 body-parser 來幫我們處理這件繁瑣且重複的事，例如可以使用 bodyParser.urlencoded 來處理用 application/x-www-form-urlencoded 格式傳輸的資料，它會將處理好的資料包進 req.body 中，如果沒有使用 parser，req.body 會是 undefined，因為原生的 http.IncomingMessage 並沒有 body。

bodyParser.urlencoded() 會回傳一個 middleware function，因此我們可以直接將其作為 app.use callback 參數，並且它似乎會在內部呼叫 next。

我們可以不指定這個 parser middleware function 的路由，這樣所有進來的 request 都會被這個 parser 處理；也可以指定在某個路由使用這個 parser，只要在我們實際要操作的 middleware function 前放置即可。