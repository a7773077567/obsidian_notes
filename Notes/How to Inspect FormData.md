---
category: note
type: tip
tags:
  - js
  - web_api
  - form
  - form_data
description: 如何印出 FormData 的內容
---
FormData 物件很龐大，為了不要消耗太多資源，瀏覽器不會讓我們直接印出來，我們只會看到其為一個 FormData 物件，但我們可以透過 for...of 加上 Object.entries() 將其遍歷打印出來

```js
const form = document.querySelector('#myForm')
const formData = new FormData(form)

for (const [key, value] of Object.entries(formData)) {
	constole.log(`${key}: ${}`)
}
```