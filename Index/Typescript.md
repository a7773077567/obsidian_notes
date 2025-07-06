---
category: index
tags:
  - ts
---

```dataview
TABLE WITHOUT ID
type as Type, rows.file.link as File, rows.description as Description
FROM #ts
WHERE category != "index"
GROUP BY type
```
