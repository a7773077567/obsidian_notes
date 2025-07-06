---
category: note
type: info
tags:
  - vue
  - router
description: 何時會觸發 Parent Vue Route 的 Hook
---
當使用者導向某個頁面時，除了本身的 exact route 會被渲染外，其所有被 match 到的 parent route 也都會被渲染，這表示其 hook 也會被呼叫，這有時候可能會導致一些非預期的狀況發生，例如當我們前往的下個頁面，其 match 到的 parent route 之 hook 有可能又重新被呼叫了一次