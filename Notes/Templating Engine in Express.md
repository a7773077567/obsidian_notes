---
category: note
type: info
tags:
  - template
  - express
  - node
  - backend
  - pug
  - ejs
  - handlebars
description: 模板引擎在 express 中的作用是什麼？
---
模板引擎 (Templating Engine) 在後端開發扮演著一個很重要的角色，他讓後端可以將資料跟模板結合生成 HTML 回應給客戶端。

當後端要回傳的 HTML 需要注入動態資料時，我們往往會使用字串拼接的方式，會很痛苦也很容易出錯，因此便有像 ejs、pug、handlebars 這類的模板引擎因孕而生。

它們可以提供近乎 HTML 寫法的體驗，並且可以動態地注入資料，這對後端的開發來說非常地方便。

大多數的模板引擎都直接支援 express，我們只需安裝其依賴並設定好 express 便可以直接使用：
```js
app.set('view engine', 'pug') // 選擇模板引擎
app.set('views', 'views') // 選擇模板引擎資料夾路徑

res.render('shop') // 透過 express 的 render 即可直接從 views 中挑選要的模板並渲染後回應給 Client
```

值得注意的是 express 可以註冊額外的引擎，例如 express-handlebars 是專門給 express 使用的：
```js
import { engine } from 'express-handlebars';

app.engine('handlebars', engine());
app.set('view engine', 'handlebars');
app.set('views', './views');
```

