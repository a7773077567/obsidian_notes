---
category: index
tags:
  - vue
---

```dataview
TABLE WITHOUT ID
type as Type, rows.file.link as File, rows.description as Description
FROM #vue
WHERE category != "index"
GROUP BY type
```
