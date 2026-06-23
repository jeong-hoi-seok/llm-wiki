---
type: index
updated: 2026-06-23
---

# Index (목차)

프론트엔드 LLM Wiki 전체 목차. 질문에 답하기 전 먼저 읽는다. 운영 규칙은 루트 `CLAUDE.md`·`AGENTS.md`.

> **플랫폼 표기** — `[공통]` 웹·앱 모두 적용 / `[웹]` Next.js 등 웹 전용 / `[앱]` React Native·Expo 전용 / `[웹·앱]` 양쪽 사용하나 구현 방식 상이

## 아키텍처

- [[FSD 폴더구조]] `[공통]` — layer·slice·segment, import rule, public API로 구조화하는 선택 가능한 아키텍처 패턴
- *(예정)* State Architecture / Data Fetching Architecture / Routing Architecture / Monorepo Architecture …

## 개발원칙

> 모두 `[공통]`

- [[코드 품질]] — 좋은 코드 4기준 허브 (토스 FF)
  - [[가독성]] · [[예측 가능성]] · [[응집도]] · [[결합도]]
- [[접근성]] — 4원칙 + 스크린리더 3요소
- [[디버깅]] — 진단 → 수정 → 예방

## 최적화

- [[프론트엔드 성능 최적화]] `[웹]` — 리소스 크기·렌더링 차단·이미지·폰트·JS 실행 비용 관리 기준

## 테스트

- [[단위 테스트]] `[웹]` — 목적·FIRST, 내부 구현 비검증, RTL 작성법, DAMP, 비용·효용
- [[통합 테스트]] `[웹]` — 경계, 컴포넌트+API, 사용자 시나리오, 단위 vs 통합 선택
- [[MSW 통합 테스트]] `[웹]` — MSW 요청 가로채기, 성공·빈·오류, 서버 상태 변경
- [[테스트 Mock·Fixture 재사용]] `[웹]` — Storybook↔테스트 handler 재사용, fixture 중복 제거, 시나리오 자산화
- [[시각적 회귀 테스트]] `[웹]` — 필요성, Playwright, 디자인 시스템 회귀, flaky 변동성 제어
- [[Storybook 기반 컴포넌트 개발]] `[웹]` — CDD, Story 관리, API 상태 표현, PR Preview 리뷰

## 기술개념

- `기술개념/web/` — 브라우저·DOM·Next.js·Tailwind·웹 번들러 중심 개념
  - **프레임워크**: [[React]] · [[Next.js]] (App Router 권장안, Pages Router 차이, 12~16.2 변화)
  - **스타일**: [[Tailwind CSS]] · [[cn & cva]]
  - **번들링**: [[번들링]] · [[트리 쉐이킹]] · [[코드 스플리팅]] · [[HMR]]
- `기술개념/app/` — React Native·Expo·NativeWind·네이티브 런타임 중심 개념
  - [[React Native]] · [[NativeWind]]
- *(예정)* Layer / Slice / Segment — FSD 세부 개념 분리 검토

## 컨벤션 (Conventions)

> 모두 `[공통]`

- [[네이밍 컨벤션]] — 변수·함수·파일 네이밍 규칙
- [[커밋 컨벤션]] — `type(scope): subject` 커밋 메시지 규칙
- [[MR PR 작성 가이드]] — GitLab MR / GitHub PR 작성 절차

## 디자인시스템

- [[AI 티 나는 디자인의 기준]] `[공통]` — AI가 만든 티가 많이 나는 디자인 패턴
- [[헤드리스 컴포넌트]] `[공통]` — 로직·접근성만 공통, UI는 위임
- [[shadcn-ui]] `[웹]` — Radix/Base UI(헤드리스) + Tailwind 복붙 소유 모델
- [[Base UI]] `[웹]` — 접근성 있는 unstyled React primitive
- *(예정)* Component API Design / Design Token / Storybook …

## 기술비교
*(필요 시 생성)*
- FSD vs Atomic Design / Headless vs Styled / Monorepo vs Polyrepo …

## 기술결정

- [[Next.js를 사용하는 이유]] `[웹]` — React 웹 앱에서 Next.js를 채택하는 기준
- [[Biome을 사용하는 이유]] `[공통]` — ESLint + Prettier 대신 Biome을 우선 검토하는 기준
- [[shadcn-ui와 Base UI 선택 기준]] `[웹]` — shadcn-ui 선택 이유와 Base UI 기반 판단 기준
- [[공통 컴포넌트 추출 기준]] `[공통]` (공통 컴포넌트 추출·유지·폐기 기준)
- [[Expo SDK 54 버전 고정]] `[앱]` (왜 Expo SDK 54.0.35로 고정하나 — Expo Go·App Store·레거시 아키텍처)
- *(예정)* Data Fetching Strategy …

## 원문요약

- [[Feature-Sliced Design 공식 문서 요약]] `[공통]` — FSD 공식 한국어 문서 요약
- [[카카오페이 공통 컴포넌트 요약]] `[웹]` — 카카오페이 공통 컴포넌트 생애주기
- 토스 Frontend Fundamentals `[웹]`:
  - [[토스 코드 품질 요약]] (좋은 코드 4기준) · [[토스 접근성 요약]] (접근성) · [[토스 번들링 요약]] (번들링) · [[토스 디버깅 요약]] (디버깅)
  - [[Frontend Fundamentals 원문 인덱스]] — 원문 135개 직접 참조 인덱스
  - [[기술 문서 작성 규칙]] `[공통]` — 기술 문서 작성 규칙 (문서 유형·구조·문장 원칙)
- [[Front-End Performance Checklist 요약]] `[웹]` — 프론트엔드 성능 체크리스트에서 범용 항목만 추린 요약
- 프론트엔드 테스트 자료 `[웹]`:
  - [[우아한형제들 단위 테스트 시리즈 요약]] · [[우아한형제들 통합 테스트 요약]] · [[카카오엔터 MSW 통합 테스트 요약]]
  - [[카카오엔터 MSW 모킹 재사용 요약]] · [[우아한형제들 시각적 회귀 테스트 요약]] · [[29CM Storybook 운영 방식 요약]]

## 템플릿

- [[AGENTS.md 가이드]] `[공통]` — 신규 프로젝트 AGENTS.md 작성 가이드 (위키 검증 장치 포함)

## 질문답변
*(필요 시 생성)*
