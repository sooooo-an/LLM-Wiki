# LLM-Wiki 셋업 기록

2026-05-03. Obsidian 기반 개인 LLM 위키를 처음부터 구축한 과정 정리.

## 출발점: LLM Wiki 컨셉

Andrej Karpathy의 아이디어. 흩어진 raw 노트를 LLM이 "컴파일"해서 서로 링크된 위키 문서로 만들어주는 패턴. 핵심은 "노트는 source code, 위키는 build artifact" 비유. 사람은 자유롭게 적기만 하고, 구조화는 LLM에게 맡긴다.

## 두 가지 접근의 충돌

이 컨셉을 실제로 구축할 때 두 철학이 부딪힌다.

- **스키마 퍼스트**: 폴더 구조, 템플릿, frontmatter 규약, Dataview 쿼리를 먼저 설계하고 그 안에 콘텐츠를 채운다. 사람이 컨텐츠 작성자.
- **콘텐츠 퍼스트**: raw 노트만 쌓아두면 LLM이 알아서 구조를 만든다. 사람은 캡처와 큐레이션만.

각자 강점이 다르다. 스키마 퍼스트는 frontmatter 일관성과 Dataview 쿼리(예: 모델 비교표)를 잘 한다. 콘텐츠 퍼스트는 노트가 쌓여도 정리 비용이 0에 수렴한다. 단점도 대칭이다 — 스키마는 손이 많이 가고, LLM 생성은 본인 스키마를 모른다.

## 하이브리드 결정

둘 다 쓴다. 단, **물리적으로 격리**한다.

- 사람이 작성하는 정식 노트: `10_Concepts/`, `20_Models/`, `30_Techniques/`, `40_Tools/`, `50_Papers/`. 본인 템플릿과 frontmatter 강제.
- LLM이 자동 생성하는 영역: `wiki/.drafts/` 안에서만 작동. 정식 폴더 절대 안 건드림.
- 승격 워크플로우: raw 캡처 → LLM 초안 → 사람 검토 → 가치 있는 것만 정식 폴더로 수동 이동 (frontmatter `source: llm-promoted` 표시)

이 격리가 깨지면 두 체계가 섞여 둘 다 망가진다.

## 폴더 구조

```
LLM-Wiki/
├── _meta/SCHEMA.md         # Vault 운영 규약
├── raw/                    # olw 입력 (사람 캡처)
├── wiki/                   # olw 출력
│   ├── .drafts/            # 검토 대기
│   ├── sources/            # 원본 추적
│   └── index.md
├── 10_Concepts/            # 정식 노트
├── 20_Models/
├── 30_Techniques/{Prompt-Engineering,RAG,Fine-tuning,Agent}
├── 40_Tools/
├── 50_Papers/
├── 60_Projects/
├── 70_Daily/
├── 80_MOCs/                # 허브 노트 (Maps of Content)
├── 90_Templates/           # Templater 템플릿
└── 99_Attachments/
```

`_meta/`는 언더스코어 prefix로 시스템 영역 표시. Templater가 스캔하지 않는 위치에 SCHEMA를 둬서 템플릿 픽커 노이즈 방지.

## 노트 타입과 스키마

5개 정식 타입 정의: concept, model, paper, technique, tool. 각각 폴더, frontmatter 필드, 필수 섹션 구조가 SCHEMA.md에 명시됨.

공통 frontmatter 필드:
- `type`: 노트 타입
- `source`: `human` | `llm-promoted` (LLM 출처와 사람 작성을 구분)

링크와 태그 규칙도 분리:
- 링크 `[[ ]]`는 모든 개념·모델·논문 연결의 기본
- 태그 `#`는 분류축이나 상태에만 (`#status/draft`, `#topic/rag`)
- 일반 명사를 태그로 쓰지 않는다 — 그건 링크의 역할

## MOC (Maps of Content)

`80_MOCs/`에 허브 노트들. 진입점인 `LLM-Wiki-Home`, 자동 인덱스인 `Models-Index`, `Papers-To-Read`, 주제 허브인 `RAG-MOC` 등. Dataview 쿼리로 frontmatter 메타데이터를 자동 집계한다.

## 도구: obsidian-llm-wiki (olw)

LLM 자동화 부분은 `obsidian-llm-wiki` (CLI 명령 `olw`) 사용. Ollama 기반으로 100% 로컬 실행.

설치: `pipx install obsidian-llm-wiki`
모델: `qwen2.5:7b` (한국어 OK, 18GB RAM 환경에 여유)

도구가 강제하는 폴더 규약 (`raw/`, `wiki/`, `wiki/.drafts/`, `.olw/`)을 그대로 채택. 처음엔 우리 `00_Inbox/`와 `05_LLM_Generated/`로 격리하려 했으나, olw 코드가 폴더 이름을 하드코딩하고 있어서 우리 쪽이 양보. 어차피 노트가 0개일 때 rename 비용이 가장 싸다.

## wiki.toml 핵심 설정

```toml
[models]
fast  = "qwen2.5:7b"
heavy = "qwen2.5:7b"

[pipeline]
auto_approve  = false
auto_commit   = false   # git 통제권은 사람이
language      = "ko"
```

`auto_commit = false`가 중요하다. olw가 임의로 git 커밋 쌓는 걸 막아야 우리 히스토리가 깨끗하게 유지된다.

## Git

Vault 자체를 git repo로 관리. `sooooo-an/LLM-Wiki` 원격 연결.

`.gitignore`로 제외:
- `.obsidian/workspace*`, `graph.json` 같은 UI 상태
- `.olw/chroma/`, `.olw/state.db` 같은 olw 로컬 DB
- 일반: `.DS_Store`, `*.log`

빈 폴더 보존을 위해 각 폴더에 `.gitkeep`. 플러그인 바이너리(`*/main.js`)는 일단 커밋 — 다른 PC에서 클론해도 환경 즉시 복원되는 이점.

## 회고: 무엇을 배웠나

1. **도구의 가정을 먼저 검증한다**. olw의 wiki.toml에 input/output 경로 필드가 있을 거라 가정했는데 실제론 없었다. README만 읽지 말고 `init` 결과물을 직접 봐야 한다.
2. **빈 폴더 = 인지 잡음**. `_meta/` 안에 `schemas/`, `fileClasses/`, `conventions/` 다 분할하려다 멈췄다. 노트 0개 상태에서 메타 스캐폴딩이 본문보다 크면 셋업 자체가 목적이 된다.
3. **격리 전략은 협상의 결과물**. 처음 설계한 격리(폴더 prefix 기반)는 도구 제약 때문에 폐기. 새 격리(폴더 이름 채택 + 정식 폴더는 도구 시야 밖)로 재설계. 격리는 한 가지 방법이 있는 게 아니라 도구·운영자의 협상이다.
4. **Templater 폴더에 SCHEMA 두지 말 것**. 템플릿 픽커가 SCHEMA를 후보로 띄워서 매번 노이즈가 된다. 매일 쓰는 명령의 마찰이 가끔 보는 문서의 co-location보다 비싸다.

## 다음 할 일

- `olw run`으로 첫 컴파일 검증 — 격리가 진짜로 지켜지는지(`10_Concepts/` 등이 안 건드려지는지) 확인
- 며칠 본인 raw 노트 쌓아본 뒤 LLM 출력 품질 평가
- `qwen2.5:7b`로 한국어 위키 품질이 부족하면 `qwen2.5:14b`로 업그레이드 검토

## 관련 키워드

LLM Wiki, Karpathy, Obsidian, Ollama, olw, hybrid approach, schema isolation, Maps of Content, Dataview, Templater, frontmatter, wikilink, RAG, fine-tuning, agent
