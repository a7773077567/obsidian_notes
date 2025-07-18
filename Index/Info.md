---
category: index
tags:
  - info
---

```dataview
TABLE WITHOUT ID
file.link as File, description as Description
WHERE category != "index" AND type = "info" AND file.name != "Prompts for AI"
SORT file.name
```
