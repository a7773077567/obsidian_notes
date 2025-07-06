---
category: note
type: tip
tags:
  - ts
  - function
  - function_overload
  - generic
description: Function overload 跟 Generic 的差別
---
在 TS 中我們可以做到兩件 JS 做不到的事情，一個是函式重載，另一個則為泛型，它們有一個相似處，就是它們可以接受不同型別的參數，並且可能會回傳不同型別的值，因此很多時候，我不太清楚這兩個的差別及它們各自的使用時機。

我們先看看什麼是函式重載：
```ts
function processValue(value: string): string // definition 1
function processValue(value: number): number // definition 2

// implementation
function processValue(value: string | number): string | number {
	if (typeof value === 'string') {
		return value.toUpperCase()
	}
	return value *2
}
```

這邊定義了兩個函式結構，而實作必須同時兼容這兩個定義，並且使用型別守衛來決定哪個型別分支會有什麼樣的行為

而泛型則會是長這樣：
```ts
function getFirstElement<T>(arr: T[]): T | undefined {
  if (arr.length === 0) {
    return undefined;
  }
  return arr[0];
}

// 此時的 T 被推論為 number
const firstNum = getFirstElement([1, 2, 3]);

// 此時的 T 被推論為 string
const firstStr = getFirstElement(['apple', 'banana', 'cherry']);
```


> [!TIP] 差在哪？
> 它們的確都收了不同型別的參數及回傳不同型別的值，而它們真正的核心差別就是**行為是否相同？**，如果仔細觀擦可以發現函式重載使用了型別守衛來 narrow down 不同的型別分支，並且給予相對應的不同行為；而泛型則是不管收了什麼型別，總是以相同的方式/行為處理這個型別。

這邊再舉一個泛型的不同型別輸入但行為相同的例子：
```ts
class StickyNoteManager<T> {
  private notes: T[] = [];

  addNote(note: T) {
    this.notes.push(note);
    console.log(`Add note: ${JSON.stringify(note)}`);
  }

  getFirstNote(): T | undefined {
    if (this.notes.length === 0) {
      console.log('No notes available');
      return undefined;
    }
    const first = this.notes[0];
    console.log(`Retrieved first note: ${JSON.stringify(first)}`);
    return first;
  }
}

const numManager = new StickyNoteManager<number>();
numManager.addNote(100);
numManager.addNote(200);
const firstNumNote = numManager.getFirstNote();
if (firstNumNote !== undefined) {
  console.log(`First number note multiply by 2: ${firstNumNote * 2}`);
}

const textManager = new StickyNoteManager<string>();
textManager.addNote('Pay the water bill');
textManager.addNote('Buy some milk');
const firstTextNote = textManager.getFirstNote();
if (firstTextNote !== undefined) {
  console.log(`First note length: ${firstTextNote.length}`);
}

interface Task {
  id: number;
  description: string;
  isDone: boolean;
}
const taskManager = new StickyNoteManager<Task>();
taskManager.addNote({ id: 1, description: 'Pay the water bill', isDone: false });
taskManager.addNote({ id: 2, description: 'Buy some milk', isDone: true });
const firstTaskNote = taskManager.getFirstNote();
if (firstTaskNote !== undefined) {
  console.log(`First todo description: ${firstTaskNote.description}`);
}
```