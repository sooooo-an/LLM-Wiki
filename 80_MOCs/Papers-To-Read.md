---
type: moc
source: human
tags: [moc, papers]
---

# Papers — To Read

`50_Papers/` 안의 논문 중 읽지 않은 것.

## To-Read 큐 (최근 추가순)

```dataview
TABLE authors, year, venue
FROM "50_Papers"
WHERE type = "paper" AND status = "to-read"
SORT file.ctime DESC
```

## 진행 중 (Reading)

```dataview
LIST
FROM "50_Papers"
WHERE type = "paper" AND status = "reading"
```

## 완료 (Done) — 최근 10개

```dataview
TABLE year, venue
FROM "50_Papers"
WHERE type = "paper" AND status = "done"
SORT file.mtime DESC
LIMIT 10
```
