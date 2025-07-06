---
category: index
tags:
  - git
---

```dataview
TABLE WITHOUT ID 
type as Type, rows.file.link as File, rows.description as Description
FROM #git
WHERE category != "index"
GROUP BY type
```
