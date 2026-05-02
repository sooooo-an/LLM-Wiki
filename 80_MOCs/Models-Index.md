---
type: moc
source: human
tags: [moc, models]
---

# Models Index

`20_Models/` 안의 정식 모델 카드 자동 인덱스.

## 전체 목록 (release 최신순)

```dataview
TABLE provider, context_window, release_date
FROM "20_Models"
WHERE type = "model" AND source = "human"
SORT release_date DESC
```

## Provider별

```dataview
TABLE WITHOUT ID file.link AS Model, context_window
FROM "20_Models"
WHERE type = "model" AND source = "human"
GROUP BY provider
SORT provider ASC
```
