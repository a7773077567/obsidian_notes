---
category: index
tags:
  - tip
---

```dataview
TABLE WITHOUT ID
file.link as File, description as Description
WHERE category != "index" AND type = "tip"
SORT file.name
```
