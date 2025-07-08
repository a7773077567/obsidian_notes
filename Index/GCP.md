---
category: index
tags:
  - gcp
---

```dataview
TABLE WITHOUT ID
type as Type, rows.file.link as File, rows.description as Description
FROM #gcp
WHERE category != "index"
GROUP BY type
```
