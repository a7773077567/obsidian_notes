---
category: note
type: problem
tags:
  - vue
  - vue-router
description: 當導向同一個頁面時，頁面並沒有重新渲染
resource_1: https://router.vuejs.org/guide/advanced/navigation-guards.html#In-Component-Guards
---
當我們使用 router.push 導向同一個頁面時，會發現頁面並沒有重新渲染，所以 setup 的 script 不會重新再被執行一次，頁面的資料也不會更新。

這是因為這個頁面有使用 path params， 因此雖然 params 更新，但實際上還是使用同一個 view，vue-router 會重複利用這個 view 實例而非銷毀它，因此最終不會重新渲染。

這段說明可以在文件的 Navigation Guard -> In-Component Guard 的 beforeRouteUpdate 的 demo 註解看到 😡：
`= this.resource_1`

有幾個方式可以解決：
1. 在 `<RouterView>` 上使用 key 屬性並綁定 $route.fullPath 來強制畫面重新渲染
2. 使用 watch 監聽 route，當 route 改變就重新獲取資料
3. 在 beforeEach hook 使用 window.location.reload() 強制 reload 頁面
4. 給一個空的類似 middleware 的頁面讓路徑不同在 redirect (不確定是否可行)