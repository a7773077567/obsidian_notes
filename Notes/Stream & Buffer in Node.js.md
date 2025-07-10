---
category: note
type: info
tags:
  - node
  - js
  - buffer
  - stream
  - data
  - chunk
  - event
description: Stream 跟 Buffer 在 Node.js 中到底是什麼東西？
---
當我們接受 HTTP request 所發送得資料時，會發現 Node.js 並不是將資料包在 req.body 或 req.data 中，而是要用 req 的 data 跟 on 事件來接收 Buffer chunk，為什麼要這麼麻煩？

這有非常多的好處，其中最大的原因就是不要阻塞整個伺服器的運作。Nodejs 將資料切成一到多段的二進位 buffer，連續不中斷地接收這些 buffer，也就是將這些資料放在一段一段的 buffer chunk 內。

我們可以透過設置 req.on('data') 來接收這些 buffer，並在接收到時做某些事，這個 data 事件會在每次收到 buffer data 時觸發。

另外我們還可以設置 req.on('end') 這個事件，當 Nodejs 注意到這個 request 的資料已經傳輸完畢時觸發。

因此我們可以在每次收到 buffer chunk 時將其放到一個陣列，等到收集完畢後再使用 Buffer.concat() 將其拼湊成一個完整的 Buffer，再將其轉換成我們要的正確資料，以下便是 Nodejs 中的標準做法：
```js
if (url === '/message' && method === 'POST') {
    const body = []
    req.on('data', (chunk) => {
      console.log(chunk);
      body.push(chunk)
    })

    req.on('end', () => {
      const parsedBody = Buffer.concat(body).toString()
      console.log(parsedBody);
      const message = parsedBody.split('=')[1]
      fs.writeFileSync('message.txt', message)
      
    })

    res.statusCode = 302
    res.setHeader('Location', '/')
    return res.end()
  }
```

另外什麼又是 Stream 呢？我們可以將 req 想像成一條河流，河流上有船隻裝載著貨物，我們不需要等到船隻全部到齊了才取用這些貨物，我們可以分批一艘一艘地取用每個抵達的貨物。

![[SCR-20250710-lmpq.png]]