---
category: note
type: info
tags:
  - node
  - js
  - event
description:
---
由於 JS 是 single-thread，為了達到 non-block 的目的，Nodejs 被設計成以事件驅動為主的程式。可想而知，Nodejs 會有非常多的 event 可以使用，我們在註冊事件的同時也會傳入非常多的 event handler callback。

但要知道的是，除了 event 以外，很多函式也是被設計成非同步的，例如 writeFile，這些函式通常也會接收 callback，在某個時間點呼叫這個 callback，通常會是某件事成功完成或者失敗後。

以下這段程式碼蠻好的示範了這種設計哲學：
```js
const server = http.createServer((req, res) => {
  const url = req.url;
  const method = req.method;

  if (url === "/") {
    res.setHeader("Content-Type", "text/html");
    res.write("<html>");
    res.write("<head><title>Enter Message</title></head>");
    res.write(
      '<body><form action="/message" method="POST"><input type="text" name="message" /><button type="submit">Submit</button></form></body>'
    );
    res.write("</html>");
    return res.end();
  }

  if (url === "/message" && method === "POST") {
    const body = [];
    req.on("data", (chunk) => {
      console.log(chunk);
      body.push(chunk);
    });

    return req.on("end", () => {
      const parsedBody = Buffer.concat(body).toString();
      console.log(parsedBody);
      const message = parsedBody.split("=")[1];
      fs.writeFile("message.txt", message, (err) => {
        res.statusCode = 302;
        res.setHeader("Location", "/");
        return res.end();
      });
    });
  }
  res.setHeader("Content-Type", "text/html");
  res.write("<html>");
  res.write("<head><title>My First Page</title></head>");
  res.write("<body><h1>Hello from my node.js Server!</h1></body>");
  res.write("</html>");
  res.end();
});

server.listen(3000);
```

首先可以看到我們在 http.createServer 丟入了一個 callback，未來它會在 Nodejs 收到 request 後被執行。

而當 url 為 `/message` 時，我們便利用 req 這個 incomingMessages stream 註冊兩個事件，分別為
 data 及 end，並且各自丟入了對應的 handler，當 server 每次接受到一個 buffer chunk 時便會發送 data 事件，並觸發對應的 handler。
 
 而當 data 已經接受完畢時便會發送 end 事件，而這個 handler 又使用了 fs.writeFile 這個非同步函式來寫入檔案，它也接收一個 callback，當寫入檔案成功或失敗時便會觸發這個 callback。
