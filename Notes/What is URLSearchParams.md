---
category: note
type: info
tags:
  - js
  - node
  - url
description: URLSearchParams 是什麼？
---
URLSearchParams 是專門設計用來處理 URL query string 的部份，它接受的是 `查詢字串格式` 的字串(USVString ?)，也就是傳入的字串只要符合 `key1=value1&key2=value2` 這個格式便可以被正確解析，如果字串包含 `?` 將會被自動忽略。

> [!WARNING] No full URLs
> URLSearchParams 並無法直接解析整串 URL，它仍然會產生物件並且不會報錯，但是只會拿到 `size: 1`，其 key 應該會是整串 URL 其值為 `null`，因為它找不到對應的 `&` 跟 `=`

因此我們應該用以下方法正確地產生 URLSearchParams 物件。

使用全域物件 Location 的 search，由於 window.location.search 回傳的便是 URL 中 query string 的部份，也就是包含 `?` 的 USVString 字串，因此可以正確被解析。

或者直接拿整串的 URL 丟給 URL constructor 來產生一個 URL 物件，而 URL 物件便有一個名稱為 searchParams 的 key，這便是一個 URLSearchParams 物件。

```js
const searchParamWithLocationSearch = new URLSearchParams(
	window.location.search
);
const searchParamWithLocationSearchName =
	searchParamWithLocationSearch.get("name");

const urlObj = new URL(window.location.href);
const searchParamsWithUrlObj = urlObj.searchParams;
const searchParamWithUrlObjName = searchParamsWithUrlObj.get("name");
```