---
type: index
updated: 2026-06-26
---

# Log Index

작업 이력 색인. 실제 ingest / query / lint 기록은 `wiki/log/YYYY-MM-DD.md`에 일자별로 남긴다.

## 작성 규칙
- 새 작업 기록은 해당 날짜 파일에 append한다.
- 날짜 파일이 없으면 `wiki/log/YYYY-MM-DD.md`를 만든다.
- `log.md`에는 날짜별 링크와 간단한 요약만 남긴다.
- `index.md`와 운영 문서가 바뀐 작업은 날짜 로그에도 함께 기록한다.

## 날짜별 로그
- [[2026-06-26]] — [[AGENTS.md 작성 가이드]] 위키 부트스트랩 방식 전환: `FRONTEND_LLM_WIKI_ROOT`+symlink resolver를 `$HOME/.llm-wiki` 단일 위치 부트스트랩(셸별 홈 절대경로 → 없으면 1회 clone·있으면 `pull --ff-only` → 절대경로로 index 열기)으로 교체 → `0.6.1`, [[위키 버전 관리]] 가독성 정리(역사적 섹션 삭제·표 슬림화 132→109행) + [[index]]에 원격 동기화 "최우선 체크" 콜아웃 추가(필수 선행 체크 격상) → `0.7.0`
- [[2026-06-25]] — 질문답변 자동 저장 지침 제거, 위키 버전 관리 컨벤션 추가, index version `0.5.0` 설정, AGENTS.md 가이드 컨벤션 이동, `원문요약/` → `summaries/` 변경, GitHub Release와 index 버전 동기화 지침 추가, 버전 올리는 기준 세분화(minor 바 상향, patch 재분류) → `0.5.1`, code-review-graph 코드 리뷰 자료 ingest + [[코드 리뷰]] 신설 → `0.5.2`, [[code-review-graph 요약]] 도구 문서 분리 신설 → `0.5.3`, app 기술개념 4종 신설([[Metro]]·[[Hermes]]·[[네이티브 모듈과 Config Plugins]]·[[Reanimated와 Gesture Handler]]) → `0.6.0`
- [[2026-06-24]] — 프로젝트 맥락 위키 작성 가이드에 raw·summaries 구조와 최신순 학습 원칙 추가, Expo 공식 docs/pages md/mdx raw 복사, Expo 개념 문서 추가, 프로젝트 맥락 위키 권장 경로를 `docs/raw`·`docs/wiki`로 보정
- [[2026-06-23]] — context 문서 frontmatter 추가, scope 규칙 구체화, 기술개념 web/app 폴더 분리, FSD 공식 KR docs raw ingest, FSD 폴더구조 재작성, 아키텍처 문서 축소, 커밋 절차 보강, context-first 작업 흐름 추가, Expo 보일러플레이트 raw 제거 / 원문요약 mandatory→조건부 결정(AGENTS ingest decoupling, 재진술 금지 규칙), 토스 FF 요약 4종 슬림화, 디자인시스템 Base UI 대폭 보강(draft→verified, 제작자·parts API·컴포넌트 40+·Radix 비교), shadcn-ui 보강(components.json·registry·eject), 테스트 문서 23→6 병합(단위·통합·MSW·Mock/Fixture·시각회귀·Storybook), MR/PR 가이드 능동 스크린샷 캡쳐 지시 추가, 프로젝트 맥락 위키 작성 가이드 신규(decisions append-only/범위 압축, worklog 온디맨드)
- [[2026-06-22]] — 프론트엔드 테스트 카테고리 추가, Front-End Performance Checklist ingest, 최적화 카테고리 추가, AI 티 나는 디자인 기준 추가, 기술결정 문서 추가, AGENTS.md 표준 운영 문서화, 아키텍처 foundation 문서 추가, 문서 제목 한글화, wiki 폴더·파일명 직관화, 로그 구조 일자별 분리
- [[2026-06-19]] — 위키 초기화, raw ingest, 카테고리 한글화, 주요 프론트엔드 문서 생성
