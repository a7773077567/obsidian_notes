---
category: note
type: problem
tags:
  - vue
  - ts
  - slot
description: 動態繼承元件 slot 時出現型別問題
resource_1: https://github.com/vuejs/core/issues/5312
---
很多時候我們會想要將某個 A 元件作為基礎，然後延伸出一個新的 B 元件，並繼承其 A 元件所有的 Slots。

例如我嘗試將 Quasar 的 QBtn 包成一個新的元件，封裝特定的樣式及 Props 並繼承 QBtn 的所有 Slots，這也就表示我必須動態地產生 Slot 並傳給 QBtn，以下是我的寫法：
```vue
<template>
	<QBtn>
		<template 
			v-for="(_, slotName) in $slots"
			:key="slotName"
			#[name]
		>
			<slot :name="slotName" />
		</template>
	</QBtn>
</template>
```

此時 IDE 並沒有顯示任何錯誤，但當我要推上遠端前，其腳本進行的 type-check 卻突然噴出了以下錯誤：
```zsh
TS7053: Element implicitly has an 'any' type because expression of type 'string | number' can't be used to index type 'QBtnSlots'.
No index signature with a parameter of type 'string' was found on type 'QBtnSlots'.
```

根據錯誤的字面意思，TS 無法在 QBtnSlots 找到 string 的 index signature，這是因為我們使用的 SlotName 是由 $slots 這個需要兼容不同可能的廣泛型別來的，所以其 index signature 應該為 string，而 QBtnSlots 的 index signature 為更精確的 'default' | 'loading' 的 union 型別，所以我們收到了此錯誤。

我們有兩個方式可以處理這個問題，其一是讓 TS 接受更寬鬆的型別，也就是想辦法讓其閉嘴；另一個更明確地告知 TS 其 index signature 為什麽，我們先看看如何寬鬆地處理：
```vue
<template>
	<QBtn>
		<template 
			v-for="(_, slotName) in ($slots as {})"
			:key="slotName"
			#[name]
		>
			<slot :name="slotName" />
		</template>
	</QBtn>
</template>
```


> [!Question] TS 為什麼閉嘴了？
> `{}` 實際上就是告訴 TS 這個物件為一個空物件，儘管它本來應該更為複雜。而 `{}` 又幾乎可以兼容任何非 null 或 undefined 的值，所以 TS 不會再去查看這個物件有什麼 index signature。因此最後它的 index signature 可以相容於任何 index signature

而另一個方式則明確很多也更推薦：
```vue
<script setup lang="ts">
defineSlots<AllSlots>();

type AllSlots = {
  [K in keyof QBtnSlots]?: Slot | undefined
};

const slots = useSlots() as AllSlots;
</script>

<template>
  <QBtn v-bind="props" padding="10px 24px" class="btn">
    <template
      v-for="(_, name) in slots "
      :key="name"
      #[name]
    >
      <slot :name="name" />
    </template>
  </QBtn>
</template>
```

這邊定義了一個新的 AllSlots，其 index signature 為 QBtnSlots 的 key union，並將 useSlots() 所回傳的 slots 斷言成 AllSlots，因此遍歷 slots 所拿到的 key，其 index signature  便會符合 QBtnSlots。


> [!TIP] 使用這個元件時可以有 slot 名稱的 hint
> 我們可以再進一步使用 defineSlots 來定義這個元件會有哪些 Slots，如果想要除了 QBtnSlots 以外的 slot，我們可以使用 & 來增加更多的 Slot 定義

