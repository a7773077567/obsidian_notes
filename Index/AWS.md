---
category: index
tags:
  - aws
---

```dataview
TABLE WITHOUT ID
type as Type, rows.file.link as File, rows.description as Description
FROM #aws 
WHERE category != "index"
SORT file.name
GROUP BY type
```
