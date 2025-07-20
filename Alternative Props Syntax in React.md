---
category: note
type: tip
tags:
  - react
  - props
  - syntax
  - js
description: 傳 props 進入 react component 的另一種技巧
---
在 React 中，所有的 Component function 都可以接受一個 props 參數 (第一個參數)，這個物件可以讓我們設定可動態傳入的資料 (props)。

既然它是一個物件表示我們也可以直接在參數的地方解構，這樣使用起來便會很直覺跟方便。

而傳入 props 的時候就跟書寫 HMTL attribute 是一樣的，只是如果要動態傳入可以使用 `{}` 取代 `""`，並在其中寫入表達式。

`{}` 可以出現在很多地方，例如 html 的 tag 中，甚至可以直接用在 custom component 上，例如以下：
```jsx
<CoreConcept {...CORE_CONCEPTS[0]} />
```

這樣便可以將這個物件的所有 fields 作為 props 傳進去此元件，類似 Vue 中的 v-bind。


> [!WARNING] Filed Name
> 自然，此物件的屬性名稱必須跟元件的名稱一致，否則會沒有作用
