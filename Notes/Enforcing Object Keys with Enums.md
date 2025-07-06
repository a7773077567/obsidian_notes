---
category: note
type: tip
tags:
  - ts
  - tip
  - object
  - enum
description: 透過 enum 限制物件的 key
---
我們很常會需要限制一個物件其 key 的範圍，例如期望這個物件只能擁有 name、id 等 key。通常我們會使用 union 來處理這個，但其實也可以使用 enum 來達到同樣的效果。

假設我們有以下 enum：
```ts
enum RoleType {
	系統管理者 = 1,
	院長 = 2,
	副院長 = 3,
}
```

我們可以透過 Record 來創建一個 key 被限制為此 enum 的 **key** 之物件，記得使用 keyof typeof 產生一個此 enum 的 key union，我們可以再透過 Partial 讓這個物件的所有 key 變為可選：
```ts
const RoleInfo: Record<keyof typeof RoleType, string> = {
	系統管理者: 'value',
	院長: 'value',
	副院長: 'value'
}

// optional
const RoleInfo: Partial<Record<keyof typeof RoleType, string>> = {
	系統管理者: 'value',
}
```

如果我們要創建的是一個 key 被限制為此 enum 的 **value** 之物件，我們可以直接將這個 enum 作為 Record 的 key type
```ts
const RoleInfo: Record<RoleType, string> = {
  1: 'value',
  2: 'value',
  3: 'value',
}

// optional
const RoleInfo: Partial<Record<RoleType, string>> = {
  1: 'value',
}
```

也可以使用 [[Mapped Types]] 加上對此 enum 使用模板字符串
```ts
const RoleInfo: { [K in `${RoleType}`]: string } = {
	1: "value",
	2: "value",
	3: "value",
}

// optional
const RoleInfo: { [K in `${RoleType}`]？: string } = {
	1: "value",
}
```