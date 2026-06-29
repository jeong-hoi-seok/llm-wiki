---
type: concept
platform: web
status: draft
tags: [frontend, testing, storybook, cdd, msw, pr-review, component]
source:
  - "[[29CM Storybook 운영 방식 요약]]"
  - "[[카카오엔터 MSW 모킹 재사용 요약]]"
created: 2026-06-22
updated: 2026-06-23
---

# Storybook 기반 컴포넌트 개발

> 컴포넌트를 독립적으로 만들고 상태를 Story로 남겨 **리뷰·테스트·문서·온보딩 자산**으로 재사용. CDD(Component-Driven Development)의 실천 도구.

## Component-Driven Development
CDD는 화면을 한 번에 만들기보다 작은 컴포넌트·상태를 독립적으로 만들고 조합해 제품 화면으로 확장하는 방식. 컴포넌트가 독립적으로 렌더·검증될 수 있어야 하고, Storybook이 대표 도구다.

29CM에서 CDD는 Storybook을 쓰며 자연스럽게 따라온다. 전체 페이지를 먼저 띄우기보다 가장 기본적·독립적인 컴포넌트부터 개발 → props 설계가 특정 페이지 맥락에 과하게 묶이는 걸 줄인다.

장점 — 중복 컴포넌트 생성 감소 / 리뷰어가 UI 동작 확인 용이 / QA·개발이 같은 상태를 기준으로 대화 / 테스트 시나리오 자산화 / 신규 입사자가 컴포넌트 목록·사용법 파악 / 디자이너가 로컬 없이 구현 상태 확인. Story 작성이 어렵다면 props API·데이터 의존이 복잡한 신호.

## 컴포넌트와 Story를 함께 관리하기
Story는 컴포넌트의 사용 예시이자 상태 문서. 29CM은 React 전환 때 Storybook을 도입하고 컴포넌트와 Story를 1:1로 맞췄다. 목적은 문서화가 아니라 팀 문제 해결 — 있는 컴포넌트를 몰라 중복 생성 / 사용법·동작 확인에 시간 / UI 테스트·리뷰에 시간.

- 컴포넌트와 Story를 1:1로. 기본·loading·empty·error·disabled·long content를 Story로.
- Story 이름은 제품 상태가 드러나게. 컴포넌트 변경 시 Story도 갱신해 사용법과 멀어지지 않게.
- 반복 boilerplate는 snippet으로. 페이지 수준 컴포넌트도 핵심 상태가 있으면 Story로. 온보딩 자료로도 관리.

## Storybook에서 API 상태 표현하기
API 컴포넌트는 props만으론 부족하다 — 응답에 따라 loading·empty·error·success가 달라진다. Storybook에서 MSW로 이 상태를 표현하면 개발 중에도 실제 화면을 빠르게 확인한다.

29CM 예: 상품 상세 이벤트 배너 영역. 실제 API에 배너가 있어도 MSW로 빈 값을 응답해 "배너 없는 상태"를 Storybook에서 확인. Storybook은 정상 데이터 카탈로그가 아니라 서버 응답별 UI 상태를 고정해 보는 환경.

Story로 남기기 좋은 상태 — 데이터 있음·없음 / 일부 필드 없음 / 이미지 없음 / 긴 텍스트 / 요청 지연 / 서버 오류 / 권한 없음.

- Story별 MSW handler 연결, 응답 상태는 제품 언어로 이름. loading·delay는 별도 Story.
- 테스트에서 같은 handler 재사용(→ [[테스트 Mock·Fixture 재사용]]). API 상태가 바뀌는 컴포넌트는 Story 없이 PR 올리지 않는 기준도 가능. 디자이너·QA·리뷰어가 이해할 이름으로.

## PR Preview 기반 UI 리뷰
스크린샷만으론 hover·focus·loading·error를 보기 어렵다. PR마다 Storybook Preview를 제공하면 리뷰어가 로컬 실행 없이 실제 상태를 보며 리뷰한다. 29CM은 GitHub Actions + Vercel Preview로 PR별 Storybook URL을 만들고 PR 코멘트에 링크를 남긴다.

- PR마다 Storybook·앱 Preview URL 생성, 주요 변경 Story 링크를 PR 설명에 첨부.
- MSW로 API 상태 재현. [[시각적 회귀 테스트]] 리포트와 함께 제공하면 효과 큼. 리뷰어가 볼 Story 이름을 PR 본문에 직접 적기.
- **주의** — Preview가 최신 커밋 기준인지 확인 / 인증·외부 API 의존성 제거 / 민감 데이터가 fixture에 안 들어가게.

## 관련 문서
- [[테스트 Mock·Fixture 재사용]] · [[시각적 회귀 테스트]] · [[MSW 통합 테스트]] · [[MR PR 작성 가이드]]

## 출처
- [[29CM Storybook 운영 방식 요약]] · [[카카오엔터 MSW 모킹 재사용 요약]]
