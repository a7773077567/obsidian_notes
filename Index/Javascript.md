---
category: index
tags:
  - js
---

```dataview
TABLE WITHOUT ID
type as Type, rows.file.link as File, rows.description as Description
FROM #js
WHERE category != "index"
GROUP BY type
```
