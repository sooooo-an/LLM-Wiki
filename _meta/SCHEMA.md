---
type: meta
source: human
tags: [meta, schema]
---

# Vault Schema

이 Vault의 노트 타입과 공통 규약 정의. **Phase 6의 LLM 도구가 초안 생성할 때 이 문서를 system prompt로 참고함.**

---

## 공통 규칙

### Frontmatter 공통 필드
모든 정식 노트는 다음 두 필드를 가짐:
- `type:` 노트 타입 (concept | model | paper | technique | tool | daily | moc)
- `source:` 출처 (`human` | `llm-promoted`)
  - `human` — 사람이 직접 작성
  - `llm-promoted` — LLM 초안에서 사람이 검토·승격
  - **이 필드 없거나 `llm-draft`면 정식 노트 아님** (`05_LLM_Generated/`에만 존재해야 함)

### 폴더 prefix 의미
- `00_` — 미분류 임시 (Inbox)
- `05_` — LLM 자동 생성 격리 영역
- `10~50_` — 정식 콘텐츠 (사람 큐레이션됨)
- `60_` — 본인 프로젝트
- `70_` — 시간 단위 노트 (daily/weekly)
- `80_` — 허브/메타 (MOC)
- `90_` — 시스템 (templates)
- `99_` — 첨부
- `_meta/` — Vault 운영 규약 (이 폴더)

### 링크 vs 태그 사용 기준
- **링크 `[[ ]]`** — 모든 개념·모델·논문·도구 간 연결의 기본
- **태그 `#`** — 분류축이나 상태에만
  - `#status/draft`, `#status/refining`, `#status/stable`
  - `#topic/rag`, `#topic/agent`
  - `#difficulty/intro`, `#difficulty/advanced`
- **금지:** 일반 명사(`#transformer`, `#gpt`)를 태그로 — 그건 링크로

### 파일명 규약
- 개념/모델/도구: `Title Case` (예: `Vector Embedding.md`, `Claude Sonnet 4.6.md`)
- 논문: `YYYY - Title (FirstAuthor et al).md` (예: `2020 - RAG (Lewis et al).md`)
- Daily: `YYYY-MM-DD.md`
- MOC: `Topic-MOC.md`

---

## Type: concept

이론·개념·아이디어 단위 문서.

- **폴더:** `10_Concepts/`
- **Frontmatter:**
  ```yaml
  type: concept
  source: human
  tags: [concept]
  created: YYYY-MM-DD
  status: draft   # draft | refining | stable
  ```
- **섹션 (필수 순서):**
  1. `## TL;DR` — 한 문장 요약
  2. `## 정의`
  3. `## 왜 중요한가`
  4. `## 어떻게 동작하는가`
  5. `## 관련 개념` — `[[wikilink]]` 최소 2개
  6. `## References`

---

## Type: model

LLM/모델 카드.

- **폴더:** `20_Models/`
- **Frontmatter:**
  ```yaml
  type: model
  source: human
  provider: <Anthropic | OpenAI | Google | Meta | ...>
  release_date: YYYY-MM-DD
  context_window: <int, 토큰 단위>
  modality: [text]   # text | image | audio | video 조합
  tags: [model]
  ```
- **섹션:**
  1. `## Spec` — Provider, Release, Context window, Pricing (input/output)
  2. `## 강점 / 약점`
  3. `## 주요 벤치마크`
  4. `## 사용 사례`
  5. `## 비교 대상` — 다른 모델 `[[wikilink]]`

---

## Type: paper

논문 리뷰.

- **폴더:** `50_Papers/`
- **Frontmatter:**
  ```yaml
  type: paper
  source: human
  authors: <쉼표 구분 또는 "First et al">
  year: YYYY
  venue: <ICLR | NeurIPS | arXiv | ...>
  url: <원문 링크>
  tags: [paper]
  status: to-read   # to-read | reading | done
  ```
- **섹션:**
  1. `## Problem` — 무슨 문제를 풀려고 하는가
  2. `## Method` — 어떻게 접근했는가
  3. `## Key Insight` — 핵심 통찰 (한 문단)
  4. `## Limitations`
  5. `## My Take` — 본인 평가 (LLM이 채우지 않음, 사람만)
  6. `## Related` — 관련 논문/개념 `[[wikilink]]`

---

## Type: technique

특정 기법·방법론 (RAG, Prompt Engineering, Fine-tuning, Agent 등).

- **폴더:** `30_Techniques/<Category>/`
  - 카테고리: `Prompt-Engineering` | `RAG` | `Fine-tuning` | `Agent`
- **Frontmatter:**
  ```yaml
  type: technique
  source: human
  category: <Prompt-Engineering | RAG | Fine-tuning | Agent>
  tags: [technique]
  status: draft   # draft | refining | stable
  ```
- **섹션:**
  1. `## TL;DR`
  2. `## 언제 쓰는가` — 적용 상황
  3. `## 어떻게 하는가` — 절차/구현
  4. `## 트레이드오프`
  5. `## 예시` — 코드 또는 케이스
  6. `## 관련` — 도구/개념 `[[wikilink]]`

---

## Type: tool

라이브러리·프레임워크·SaaS (LangChain, Ollama, vLLM, Pinecone 등).

- **폴더:** `40_Tools/`
- **Frontmatter:**
  ```yaml
  type: tool
  source: human
  category: <framework | runtime | vector-db | observability | ...>
  language: <Python | TypeScript | Rust | ...>
  url: <공식 사이트 또는 repo>
  tags: [tool]
  ```
- **섹션:**
  1. `## 무엇인가`
  2. `## 핵심 기능`
  3. `## 설치·시작`
  4. `## 사용 예`
  5. `## 대안 / 비교` — 경쟁 도구 `[[wikilink]]`
  6. `## 한계·주의점`

---

## Type: daily (참고)

자유 형식. 템플릿(`90_Templates/Daily-Template.md`)에 의존. LLM 도구가 다루지 않음.

## Type: moc (참고)

자유 형식. 허브 노트라 구조 강제 안 함. `_meta/` 또는 LLM 도구 입력에서 제외.

---

## LLM 도구가 지켜야 할 제약 (Phase 6)

Phase 6에서 `obsidian-llm-wiki-local` 도구가 초안을 만들 때:
1. 출력은 무조건 `05_LLM_Generated/` 안에만. 정식 폴더(10~50) 절대 건드리지 않음
2. 생성한 frontmatter는 `source: llm-draft`로 표시 (사람이 승격 시 `llm-promoted`로 변경)
3. `## My Take` 같은 "사람만 채우는 섹션"은 빈 칸으로 두고 채우지 않음
4. 위키링크 `[[ ]]`는 자유롭게 생성하되, 깨진 링크 OK (사람이 승격하면서 정리)
