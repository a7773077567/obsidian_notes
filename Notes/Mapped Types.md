---
category: note
type: info
tags:
  - ts
description: 映射型別有哪些？其作用是什麼？
---
映射型別 Mapped Types 是 TS 中很強大的一個特性，它可以讓我們基於現有的型別，用映射的方式來建立一個新的型別。

它會用到一個特殊的語法，類似 JS 中 for...in 來遍歷物件的 key，它非常方便。舉例來說，我們很常想要限制某個物件型別其 key 在某個範圍，而這個範圍便是一個集合，也就是一個 union，我們可以這樣寫：
```ts
const keys = ['pedro', 'anita', 'alex'] as const

type People = { [K in typeof keys[number]]: string }
```

除了使用 in 這個 keyword 來進行遍歷的映射外，TS 還供了各種內建的映射型別，例如以下：
- Partial
- Readonly
- Pick
- Omit
- Record