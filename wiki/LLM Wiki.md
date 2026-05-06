---
confidence: 0.5
created: '2026-05-06'
sources:
- raw/Obsidian으로 LLM 위키 만들기.md
status: published
tags:
- llm-wiki
title: LLM Wiki
updated: '2026-05-06'
---


## LLM 위키란?

LLM 위키는 문서를 읽을 때마다 지식을 축적하고, 질문 시 그 축적된 결과물을 사용하는 구조입니다. 주요 특징은 다음과 같습니다.

- **원문 소스(Raw sources)**: 변경 불가능한 문서 컬렉션으로, LLM은 이 레이어를 읽기만 하고 수정하지 않습니다.
- **위키(The wiki)**: LLM이 생성하는 마크다운 파일 디렉터리로, 요약, 엔티티 페이지, 개념 페이지 등을 포함합니다. 사용자는 이 레이어를 읽지만, 작성은 LLM이 담당합니다.
- **스키마(The schema)**: 위키 구조와 컨벤션을 정의하는 설정 문서입니다. `CLAUDE.md` 같은 파일로 구성됩니다.

## 적용 분야
LLM Wiki는 다양한 분야에서 활용될 수 있습니다:

- 개인: 목표, 건강, 심리, 자기개발 추적 - 저널, 기사, 팟캐스트 노트를 구조화된 자아 기록으로 변환합니다.
- 연구: 논문, 보고서를 읽으며 진화하는 테제를 담은 포괄적인 위치 구축이 가능합니다.
- 독서: 챕터별로 정리하며 등장인물, 테마, 플롯 실을 페이지로 구성합니다.
- 비즈니스/팀: 슬랙 스레드, 미팅 전사, 프로젝트 문서, 고객 통화를 통해 LLM이 유지 관리하는 내부 위키 구축이 가능합니다.

## 주요 작업
주요 작업은 다음과 같습니다:

- **인제스트(Ingest)**: 새 소스를 원문 컬렉션에 추가하고 LLM에게 처리를 지시합니다. 이 과정에서 핵심 내용을 추출하고 요약 페이지를 작성하며, 관련 엔티티/개념 페이지를 수정하고 인덱스를 업데이트합니다.
- **쿼리(Query)**: 질문 시 별도의 대규모 검색이 필수는 아닙니다. 이미 정리된 위키 중심으로 답변을 생성합니다.
- **린트(Lint)**: 주기적으로 LLM에게 위키 상태 점검 요청을 합니다. 모순되는 페이지, 링크가 없는 고아 페이지 등을 확인하고 수정합니다.

## 인덱싱 및 로깅
인덱스와 로그는 다음과 같이 구성됩니다:

- **index.md**: 위키의 수동/텍스트 기반 검색 인덱스입니다. 질문 시 이를 먼저 읽고 관련 페이지 후보를 찾습니다.
- **log.md**: 시간순으로 기록된 인제스트, 쿼리, 린트 통과 내역을 순서대로 기록합니다.

## 팁 및 도구 활용법
Obsidian Web Clipper를 사용하여 웹 기사를 마크다운으로 변환하고, 로컬 이미지를 저장할 수 있습니다. Obsidian 그래프 뷰를 통해 위키 전체 형태를 파악하며, Marp와 Dataview 등의 플러그인을 활용해 더욱 효과적으로 작업합니다.

## LLM Wiki 만들기
1. **Obsidian 설치하기**
2. [https://ollama.com/](https://ollama.com/) 설치하기
3. [https://github.com/kytmanov/obsidian-llm-wiki-local](https://github.com/kytmanov/obsidian-llm-wiki-local) 설치하기
4. Ollama에서 모델 설치하기
5. 명령어를 이용하여 wiki 설정해주기
6. 작성된 초안을 raw에 올리기 
7. `olw run` 명령으로 위키 생성하기:
   - `olw ingest --all`
   - `olw compile`
8. **wiki/.drafts/**: LLM이 만든 초안 검토 대기
9. `olw review (approve/reject/edit)`
10. wiki/: 승인된 위키 본문 
11. 사람의 판단으로 정식 노트로 이동시 템플릿 적용하여 정리하기
12. 정식 노트로 이동 후 LLM이 안건을 건드릴 수 없습니다.
[[RAG (Retrieval-Augmented Generation)|RAG]]
[[Operation Log]]
[[Obsidian으로 LLM 위키 만들기|LLM Wiki Creation Guide]]

## Sources
- [[Obsidian으로 Llm 위키 만들기]]

## See Also
- [[Obsidian으로 LLM 위키 만들기]]
- [[Operation Log]]
- [[RAG (Retrieval-Augmented Generation)]]