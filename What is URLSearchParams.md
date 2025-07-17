---
category: note
type: info
tags:
  - js
  - node
  - url
description: URLSearchParams 是什麼？
---
URLSearchParams 是專門設計用來處理 URL query string 的部份，當我們將 URL 字串生成的 URL 物件傳入便可以創建一個 URLSearchParams 物件。

但其實 URLSearchParams 還可以接受 `查詢字串格式` 的字串(USVString ?)，也就是傳入的字串符合 `key1=value1&key2=value2` 這個格式便可以被正確解析，如果字串包含 `?` 也會被自動忽略。

