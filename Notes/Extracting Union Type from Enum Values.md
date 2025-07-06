---
category: note
type: tip
tags:
  - ts
  - enum
  - union
description: 取得 enum values 的 union type
---
我們很常會透過定義 enum 來固定某些常數，將其當作一張表來查閱跟取用，例如某個角色其對應的數字。

但有時候我們會需要取用這個 enum 的值來做為某個物件的 key，所以我們必須想辦法先取得這個 enum values 的 union，最直接的方式就是使用模板字符串，例如：
```ts
num Weekday {
    MONDAY = 'mon',
    TUESDAY = 'tue',
    WEDNESDAY = 'wed'
}

type WeekdayType = `${Weekday}`
// type WeekdayType = "mon" | "tue" | "wed"
```

> [!INFO] 當 enum 的值為 number 時
> 使用模板字符串可以將 enum 的值變成數字字串的 union


從 [Stack Overflow](https://stackoverflow.com/questions/52393730/typescript-string-literal-union-type-from-enum) 上可以看到有人表示可以使用物件來做到這件事，而非使用 enum：
```ts
const Weekday = {
  MONDAY: "mon",
  TUESDAY: "tue",
  WEDNESDAY: "wed",
} as const;

type Weekday = (typeof Weekday)[keyof typeof Weekday];
```