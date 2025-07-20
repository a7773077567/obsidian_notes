---
category: note
type: info
tags:
  - js
  - react
  - framework
description: 學習 React.js 的一些雜記
---
## Concepts

- React 是一種 Declarative UI Programming，我們只需要定義 UI 的目標狀態即可，而非定義每個步驟來達成最後的結果，React 可以理解我們的目標並幫我們執行所有必要的步驟
- 跟大多數的框架一樣，React 透過一個 entry point 將整個 component tree 注入頁面中。
	- 首先我們可以看到 index.html 只會有一個 id 為 root 的 div 元素，跟一個其 src 為 `/src/index.js` 的 script。
	- 在 index.js 中，我們引入了 ReactDOM 這個物件，並取得 root Dom 將其傳入 createRoot  method 作為 entry point，接著再使用 createRoot 回傳之物件地 method 來傳染整個 component tree，也就是 App.jsx。
- React 會遍歷整個 component tree 並將所有的 custom component 轉換成 built-in component (html element)，直到沒有任何的 custom component剩下，才會渲染畫面。
- 我們可以在 custom component 中丟入其他 element 或 content，稱之為 component composition，就像 Vue 的 slot 一樣。
	- 這個 element 或 content 會被收集進 props.children
	- 目前看起來 React 只能傳入一個 children
- React 在渲染所有元件時，實際上只會渲染一次，也就是元件 function 只會被執行一次，我們可以在元件內的 top-level 寫 log 來驗證這件事。