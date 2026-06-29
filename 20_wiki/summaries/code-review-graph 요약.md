---
type: source-summary
scope: code-review-graph-tool
platform: common
status: verified
tags: [frontend, code-review, tooling, ai, mcp]
source: 10_raw/code-review-graph/
created: 2026-06-25
updated: 2026-06-25
---

# code-review-graph 요약

## 원문 정보
- 도구: `code-review-graph` (오픈소스, MIT)
- 저장소: https://github.com/tirth8205/code-review-graph
- 웹: https://code-review-graph.com
- 원본: `10_raw/code-review-graph/README.ko-KR.md`, `10_raw/code-review-graph/skills/*.SKILL.md`
- 슬로건: "토큰 낭비를 멈추세요. 더 스마트하게 리뷰하세요."

> 리뷰 **원칙**은 [[코드 리뷰]] 참고. 이 문서는 그 원칙을 구현한 **도구**의 동작·기능·사용법 정리.

## 한 줄 요약
AI 코딩 도구가 리뷰 때 코드베이스를 통째로 반복해 읽어 토큰을 낭비하는 문제를 해결하는 도구. 코드를 구조 그래프로 만들어 두고, 변경 시점에 **읽어야 할 최소 파일 집합**만 계산해 MCP로 AI에게 넘긴다. 실제 저장소 기준 평균 8.2배(최대 27배) 토큰 절감. [검증 필요] 수치는 도구 자체 평가.

## 작동 원리

```txt
저장소 → Tree-sitter 파싱(AST) → SQLite 그래프 → 변경 시 쿼리 → 최소 리뷰 세트
```

- **그래프 구조**: 노드 = 함수·클래스·import / 엣지 = 호출·상속·테스트 커버리지.
- **저장**: `.code-review-graph/`에 SQLite. 외부 DB·클라우드 불필요(로컬).
- **영향 범위(blast radius)**: 파일이 바뀌면 호출자·의존 대상·테스트를 추적해 영향 범위를 계산. AI는 전체 스캔 대신 이 파일만 읽는다.
- **점진적 업데이트**: 파일 저장·git 커밋 훅 또는 watch 모드로 변경 파일만 SHA-256 비교 후 재파싱. 2,900개 파일 재인덱싱이 2초 이내.
- **모노레포 효과**: 27,700개 파일 중 실제로 읽는 건 약 15개까지 좁힘 — 토큰 낭비가 큰 대규모 레포일수록 이득.

## 주요 기능

- **영향 범위 분석** — 변경에 영향받는 함수·클래스·파일.
- **위험 점수 리뷰** (`detect_changes`) — diff를 영향 함수·실행 흐름·테스트 격차에 매핑.
- **시맨틱/하이브리드 검색** — 벡터 임베딩(sentence-transformers·Gemini·OpenAI 호환) + FTS5 키워드. 함수 시그니처만 임베딩(노드당 약 10토큰).
- **구조 분석** — 허브/브릿지 노드(병목), 커뮤니티 감지(Leiden), 지식 격차(테스트 안 된 핫스팟), 서프라이즈 스코어링(예상 못 한 결합).
- **리팩토링** — 이름 변경 미리보기, 데드 코드 감지, 커뮤니티 기반 제안.
- **시각화·내보내기** — D3.js 인터랙티브 그래프, GraphML / Neo4j Cypher / Obsidian vault / SVG.
- **위키 생성** — 커뮤니티 구조에서 마크다운 위키 자동 생성.
- **멀티 레포 레지스트리** — 여러 저장소 등록 후 교차 검색.

## 폭넓은 언어 지원
Python, JS/TS/TSX, Go, Rust, Java, C/C++, C#, Ruby, Kotlin, Swift, PHP, Scala, Solidity, Dart, R, Perl, Lua, shell, Elixir, Zig, PowerShell, SQL, Vue/Svelte SFC, Jupyter(`.ipynb`) 등. Tree-sitter 우선 + 대상별 fallback 파서.

## 설치·사용

```bash
pip install code-review-graph        # 또는 pipx / uvx
code-review-graph install            # AI 코딩 도구 자동 감지 + MCP 설정 주입
code-review-graph build              # 코드베이스 파싱 (500파일 약 10초)
```

`install`은 Codex·Claude Code·Cursor·Windsurf·Zed·Gemini CLI·Copilot 등 설치된 도구를 감지해 각각에 MCP 설정과 그래프 인식 지침을 넣는다. Python 3.10+ 필요. `uv` 권장.

### 슬래시 명령 (AI 도구 내)
- `/code-review-graph:build-graph` — 그래프 빌드·재빌드
- `/code-review-graph:review-delta` — 마지막 커밋 이후 변경 리뷰
- `/code-review-graph:review-pr` — 영향 범위 포함 전체 PR 리뷰

### MCP 통합
그래프 빌드 후 AI가 30개 MCP 도구를 자동 사용. 토큰 효율 순서:
1. `get_minimal_context_tool` — 초소형 컨텍스트(~100토큰), **먼저 호출**.
2. `get_review_context_tool` / `get_impact_radius_tool` — 리뷰 컨텍스트·영향 범위.
3. `query_graph_tool` — 호출자·피호출자·테스트·import·상속 쿼리.

MCP 프롬프트 5종: `review_changes`, `architecture_map`, `debug_issue`, `onboard_developer`, `pre_merge_check`.

## 언제 쓰나 / 한계
- **적합**: 대규모·모노레포에서 AI 리뷰 토큰 비용이 클 때, 변경 영향 범위를 자동으로 좁히고 싶을 때.
- **한계**: 현재 임베딩은 **함수 시그니처만** 처리(body·docstring 미포함) — 의미 검색 정밀도 한계. `-preview`/`-beta` 모델은 장기 운영 비권장(가중치 변경 시 전체 re-embed). [검증 필요] body 임베딩은 후속 과제로 추적 중.

## 관련 문서
- [[코드 리뷰]] — 리뷰 원칙(영향 범위·위험도·구조화 출력)
- [[디버깅]] · [[MR PR 작성 가이드]]
