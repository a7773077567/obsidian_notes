---
category: index
tags:
  - node
---

```dataview
TABLE WITHOUT ID
type as Type, rows.file.link as File, rows.description as Description
FROM #node 
WHERE category != "index"
GROUP BY type
```
