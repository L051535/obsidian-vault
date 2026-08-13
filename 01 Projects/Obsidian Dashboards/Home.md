---
tags:
  - dashboard
date: 2026-04-02
status: active
type:
---
# Home

## Active Projects

```dataview
LIST
FROM "01 Projects"
WHERE file.name != "Home" AND file.name != "Obsidian Dashboard Index"
SORT file.mtime DESC
```

## Open Tasks

```tasks
not done
sort by due
limit 15
```

## Recently Edited

```dataview
TABLE file.mtime AS "Modified"
FROM ""
SORT file.mtime DESC
LIMIT 10
```

## Jump To

- [[PARA Framework]] · [[My Note-Taking System]]
- [[Projects Index]] · [[Archive Index]]
- [[NALO Trainings MOC]]
- [[Obsidian Dashboard Index]]
