---
category: note
type: info
tags:
  - ts
  - function
description: 定義 function signature 的方式
---
在 Typescript 中，定義 function signature 有幾種方式：
1. 使用 type alias
	1. 直接定義函式結構
	2. 使用物件形式
2. 使用 interface 
3. 透過 function overload 並實作其函式

使用 type alias 直接定義函式結構：
```ts
type MathOperation = (x: number, y: number) => number

const add: MathOperation = (x: number, y: number) => x + y
const subtract: MathOperation = (x: number, y: number) => x + y
```

> [!NOTE] 使用函式表達式
> 當我們想要將某個函式標註為某個函式型別時必須使用函式表達式，而非函式宣告

使用物件的方式來定義一個函式型別：
```ts
type MathOperation = {
	(x: number, y: number): number
	id: number
}
```

使用 interface 來定義一個函式型別：
```ts
interface MathOperation {
	(x: number, y: number): number
	id: nubmer
}
```

> [!QUESTION] 為什麼可以用物件的形式來定義函式
> 在 JS 中，函式是一個擁有可呼叫行為(internal mathod)的特殊物件，因此它可以使用 type alias 的物件型別定義，亦或是使用  interface 來定義都行，並且還可以定義其屬性

透過 function overload 來定義：
```ts
function processValue(value: string): string
function processValue(value: number): number

function processValue(string | number): string | number {
	if (typeof value === 'string') {
		return value.toUpperCase()
	}
	return value * 2
}
```

這邊定義了 2 個函式結構，然後根據以上的定義實作了一個函式，也就是這個實作的函式必須同時相容這 2 個定義，不然 TS 會告訴你其中的某個定義並沒有符合實作的函式。


> [!TIP] 行為不同
> 可以發現這邊使用型別守衛來進行 narrow down，並且每個分支會有不同的行為，也就是根據輸入類型的不同，有相對應的不同行為，可以參考 [[Function Overload vs. Generic in Typescript]]
