---
aliases:
- RAG
- Retrieval Augmented Generation
confidence: 0.5
created: '2026-05-06'
sources:
- raw/Obsidian으로 LLM 위키 만들기.md
status: published
tags:
- llm-wiki
- retrieval-augmented-generation
- artificial-intelligence
- language-models
title: RAG (Retrieval-Augmented Generation)
updated: '2026-05-06'
---

# RAG (Retrieval-Augmented Generation)


## Retrieval-Augmented Generation 방식
**RAG(Retrieval-Augmented Generation)**는 LLM이 질문에 대답하기 위해 외부 문서를 검색하고 답변을 생성하는 방법이다. 이 방식은 다음과 같은 단계로 구성된다.

1. 문서 업로드
2. 문서 쪼개기(chunking)
3. 각 chunk → 임베딩 생성
4. Vector DB에 저장

질문 시에는 다음과 같은 과정을 거친다.

5. 질문 → 벡터 전환
6. 유사한 텍스트 조각(chunk) 검색
7. (질문 + chunk) → LLM 입력
8. LLM이 답변 생성

## RAG의 주요 특징
- **Retrieval**
  - 질문(query)을 벡터로 변환
  - 문서 DB(백터 DB)에서 유사한 텍스트 조각(chunk)을 검색
- **Generation**
  - 검색된 chunk들을 LLM에 함께 넣어서
  - 그 내용을 근거로 답변 생성

## LLM Wiki와의 차이점
LLM Wiki는 RAG 방식과 달리 문서를 읽을 때마다 지식을 축적하고, 정리하여 질문 시 이전에 저장된 결과물을 사용한다. 이렇게 하면 별도 대규모 검색이 필요하지 않으며, 답변 생성 과정에서 더 나은 품질의 결과를 얻을 수 있다.

## [[LLM Wiki]] 구조
LLM Wiki는 다음과 같은 세 가지 레이어로 구성된다:

- **원문 소스(Raw sources)**: 변경 불가능한 문서 컬렉션. 이를 통해 진실의 원천(source of truth)을 제공한다.
- **위키(The wiki)**: LLM이 생성하는 마크다운 파일 디렉터리로, 요약, 엔티티 페이지, 개념 페이지 등을 포함한다.
- **스키마(The schema)**: 위키 구조와 워크플로를 설정하는 문서. 이 설정은 사용자와 LLM의 상호작용을 통해 점진적으로 개선된다.

## 주요 작업(Operations)
LLM Wiki는 다음과 같은 주요 작업들을 수행한다:

- **인제스트(Ingest)**: 새 소스를 위키에 추가하고 관련 페이지를 생성 및 업데이트한다. 이를 위해 `index.md`와 `log.md`도 관리된다.
- **쿼리(query)**: 질문 시 이미 정리된 위키에서 답변을 찾는다. 이렇게 하면 LLM이 모든 문서를 매번 검색하지 않아도 된다.
- **린트(Lint)**: 위키의 상태 점검을 수행하여 오류와 불일치를 수정한다.

## 인덱싱 및 로깅
인덱스와 로그는 다음과 같은 역할을 한다:

- `index.md`: 위키 전체 구조를 텍스트 기반으로 검색하기 위한 도구로 사용된다. 질문 시 이 파일을 먼저 읽어 관련 페이지 후보를 찾고, 해당 페이지에서 답변을 생성한다.
- `log.md`: 인제스트, 쿼리, 린트 작업의 시간순 로그를 저장하여 추적한다.

## 팁 및 도구 활용법
LLM Wiki 구축과 관리를 위한 몇 가지 도구와 팁:

- Obsidian Web Clipper: 웹 기사를 마크다운으로 변환
- Marp: 슬라이드 덱을 만드는 플러그인
- Dataview: 페이지 프론트매터를 대상으로 쿼리를 실행하는 플러그인
- Obsidian 그래프 뷰: 위키 전체 구조 파악

## LLM Wiki 만들기 단계
1. Obsidian 설치하기 Obsidian
2. [ollama.com](https://ollama.com/) 설치하기
3. [kytmanov/obsidian-llm-wiki-local]((https://github.com/kytmanov/obsidian-llm-wiki-local)) 설치하기
4. Ollama에서 모델 설치하기
5. 명령어를 이용하여 wiki 설정해주기
6. 작성된 초안을 `raw`에 올리기
7. olw run을 통해 wiki 생성하기:
   - olw ingest —all: 모든 문서 인제스트
   - olw compile: 위키 컴파일 및 생성
8. `wiki/.drafts/`: LLM이 만든 초안 검토 대기
9. olw review (approve/reject/edit): 초안 검토 및 승인
10. `wiki/`: 승인된 위키 본문
11. 정식 노트로 이동 시 템플릿 적용 및 정리하기

[[Operation Log]], [[Wiki Index]]

## Sources
- [[Obsidian으로 Llm 위키 만들기]]

## See Also
- [[LLM Wiki]]
- [[Operation Log]]
- [[Wiki Index]]