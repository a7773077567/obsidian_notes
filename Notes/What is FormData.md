---
category: note
type: info
tags:
  - js
  - node
  - form
description: FormData 是什麼？
---
首先要知道 FormData 並不是一種資料傳輸的 Content-Type，它是一種 JS 物件，它的存在是方便我們操作 Form 的資料。

FormData 可以作為  fetch 或 XMLHttpRequest 的 body，並且瀏覽器會自動將其 request header 的 Content-Type 設為 multipart/form-data 及把資料編譯，我們不需要再手動設定。

我們可以將 Form 元素直接丟進 new FormData()，這樣便會產生一個 FormData 物件，我們可以動態地使用 append 來添加資料，並且將其作為 fetch 或 XMLHttpRequest 的 body 使用。

```js
// HTML
// <form id="myForm">
//   <input type="text" name="username" value="Alice">
//   <input type="file" name="profilePic">
//   <button type="submit">Submit with JS</button>
// </form>

const formElement = document.getElementById('myForm');

formElement.addEventListener('submit', function(event) {
  event.preventDefault(); // 阻止表單的默認提交行為 (即阻止頁面跳轉)

  const formData = new FormData(formElement); // 透過 JS 建立 FormData 物件

  // 你可以向 formData 添加額外的數據
  formData.append('source', 'javascript');

  fetch('/upload-js', {
    method: 'POST',
    body: formData // 直接傳入 FormData 物件
  })
  .then(response => response.text())
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));
});
```



