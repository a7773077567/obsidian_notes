---
category: index
tags:
  - react
---

```dataview
TABLE WITHOUT ID
type as Type, rows.file.link as File, rows.description as Description
FROM #react 
WHERE category != "index"
GROUP BY type
```
