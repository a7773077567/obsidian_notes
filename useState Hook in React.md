---
category: note
type: info
tags:
  - react
  - state
  - js
description: React 中的 useState hook
---
React 在渲染所有元件時，實際上只會渲染一次，也就是元件 function 只會被執行一次，我們可以在元件內的 top-level 寫 log 來驗證這件事，所以當我們將某個變數綁定至 jsx 上，我們更新這個變數時，UI 並不會重新渲染。

如果要做到這件事，我們必須透過 useState 給這個元件一個 state，當這個 state 變動時，元件就會被重新渲染一次。

useState 是一個 React hook function，它在執行的時候必須傳入一個 initial value 作為最初的 state，並且它會回傳一個包含 state 及 setState function 的 tuple。

如果這個 state 有被綁定至 jsx，那麼當這個 state 變動，元件 function 就會被重新執行一次，而這個變動應該要透過 setState 來處理，因為只有使用 setState 才會觸發元件重新渲染。

setState 後的 UI 改動應該是一個非同步 function，因此在 setState 之後的程式碼也會被執行，


> [!WARNING] LIMITATIONS 
> - useState 只能在 function component 中使用
> - 必須是在 top -level，不能在任何 block 或 function 內
