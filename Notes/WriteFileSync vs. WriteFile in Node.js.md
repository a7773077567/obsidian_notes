---
category: note
type: info
tags:
  - node
  - js
  - fs
  - async
description: WriteFileSync 跟 WriteFile 在 Nodejs 中的差別
---
按照字面上的意思來看， WriteFileSync 是同步，而 WriteFile 是非同步。由於 Nodejs 是一個事件驅動 (event driven) ([[Understanding Nodejs's Event-Driven Architecture]]) 的程式，因此可以大膽猜測 WriteFile 會需要收一個 callback handler 的參數，它會在寫入檔案成功或者失敗後呼叫，而當錯誤產生時，這個 callback 就會被丟入一個 err 物件執行。

我們永遠不會知道傳輸過來的資料會有多大，因此非同步地寫入檔案顯得格外重要，這樣才不會影響程式繼續往下執行，甚至是阻塞其他 request 的接收跟執行。

```js
fs.writeFile("message.txt", message, (err) => {
	res.statusCode = 302;
	res.setHeader("Location", "/");
	return res.end();
});
```