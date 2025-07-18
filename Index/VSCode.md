---
category: index
tags:
  - vscode
---
```dataview
TABLE WITHOUT ID
type as Type, rows.file.link as File, rows.description as Description
FROM #vscode 
WHERE category != "index"
SORT file.name
GROUP BY type
```
