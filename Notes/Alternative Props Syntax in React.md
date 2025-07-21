---
category: note
type: tip
tags:
  - react
  - props
  - syntax
  - js
description: 傳 props 進入 react component 的另一種技巧
---
在 React 中，所有的 Component function 都可以接受一個 props 參數 (第一個參數)，這個物件可以讓我們設定可動態傳入的資料 (props)。

既然它是一個物件表示我們也可以直接在參數的地方解構，這樣使用起來便會很直覺跟方便。

而傳入 props 的時候就跟書寫 HMTL attribute 是一樣的，只是如果要動態傳入可以使用 `{}` 取代 `""`，並在其中寫入表達式。

`{}` 可以出現在很多地方，例如 html 的 tag 中，甚至可以直接用在 custom component 上，例如以下：
```jsx
<CoreConcept {...CORE_CONCEPTS[0]} />
```

這樣便可以將這個物件的所有 fields 作為 props 傳進去此元件，類似 Vue 中的 v-bind。


> [!WARNING] Filed Name
> 自然，此物件的屬性名稱必須跟元件的名稱一致，否則會沒有作用

除了使用展開運算子來傳入一個物件 props，也可以使用 rest operator 來收集 props，例如以下：

```jsx
<CoreConcept
  title={CORE_CONCEPTS[0].title}
  description={CORE_CONCEPTS[0].description}  
  image={CORE_CONCEPTS[0].image}
/>
```

```jsx
export default function CoreConcept({ ...concept }) { 
  // ...concept groups multiple values into a single object
  // Use concept.title, concept.description etc.
  // Or destructure the concept object: const { title, description, image } = concept;
}
```

這邊的 `{ ...concept }` 是解構賦值一個很進階的用法，來簡單拆解一下：
- 首先，所有傳入 CoreConcept 的 props 都會被收集成一個物件並作為第一個參數傳入 CoreConcept
- 接著我們將第一個參數(props 物件) 進行解構賦值，此時可以將此物件視為展開的狀態，類似這種概念
	- `title: "Title", description: "Description...", image: "the/image/path"`
-  此時我們便可以馬上再利用 rest property 將它們收集成一個變數 - `concept` 進而利用

這個做法在實務上可能有其很方便的地方，例如我們可以把某部份的 props 收集起來直接往下傳遞：
```jsx
function Caption({ caption, color }) {
  return <p style={{ color }}>{caption}</p>;
}

function CoreConcept({ image, title, description, ...captionProps }) {
  return (
    <li>
      <img src={image} alt={title} />
      <h3>{title}</h3>
      <p>{description}</p>
      <Caption {...captionProps} />
    </li>
  );
}

<CoreConcept
	{...CORE_CONCEPTS[0]}
	caption="Caption"
	color="hotpink"
/>
```

這也算是一種關注點分離，我們只關注跟 CoreConcept 有關的 Props，其餘的則直接收集起來往下傳遞即可。

另外我們也可以給予在解構賦值的同時給予預設參數，例如以下：
```jsx
function Caption({ caption, color = 'hotpink' }) {
  return <p style={{ color }}>{caption}</p>;
}

function CoreConcept({ image, title, description, ...captionProps }) {
  return (
    <li>
      <img src={image} alt={title} />
      <h3>{title}</h3>
      <p>{description}</p>
      <Caption {...captionProps} />
    </li>
  );
}

<CoreConcept
	{...CORE_CONCEPTS[0]}
	caption="Caption"
/>
```