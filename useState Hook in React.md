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

setState 後的 UI 改動應該是一個非同步 function，re-render 會進入排隊，等待適當的時機進行渲染，因此在 setState 之後的程式碼也會馬上被執行，全部執行完後才會被 return。

> [!WARNING] LIMITATIONS 
> - useState 只能在 function component 中使用
> - 必須是在 top -level，不能在任何 block 或 function 內

useState 有一個非常重要的概念，就是 snapshot，每個 jsx 就像一張可互動的快照。當我使用 setState 告訴 React 我要換一張快照時，它就會重新執行 component function，並且根據當時的 state 產生一張新的 jsx 快照，React 會想辦法讓現在的 UI 去 match 這張新的快照，最後便根據這張快照重新渲染了畫面。

state 並非是一般的 JS 變數，它被 React 儲存在 function component 的 scope 外，我們在 function component 內拿到的 state 是一個當下的 copy，我們在同一個 stack 內的 state 是不會變的，因此我們在 setState 後馬上 log state 得到得仍然會是原本的 value。

這也就產生了一個問題，如果我們在一個 handler 內同時呼叫多次 setState 很有可能跟呼叫一次的結果是一樣的，因為我們一直在使用相同的 state value 在操作，例如以下雖然執行了 3 次 setState，但都是渲染 3 次同樣的結果：
```jsx
<button onClick={() => {
  setNumber(number + 1);
  setNumber(number + 1);
  setNumber(number + 1);
}}>+3</button>
```


> [!TIP] Tip
> 簡單來說，state 在同一次的渲染內是不會改變的





