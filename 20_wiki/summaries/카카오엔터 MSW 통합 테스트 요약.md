---
type: source-summary
platform: web
status: summarized
tags: [frontend, testing, integration-test, msw]
source: "https://tech.kakaoent.com/front-end/2022/220825-msw-integration-testing/"
created: 2026-06-22
updated: 2026-06-22
---

# 카카오엔터 MSW 통합 테스트 요약

## 한 줄 요약
카카오엔터테인먼트 MSW 통합 테스트 글은 API 요청이 있는 프론트엔드 화면에서 성공, 빈 응답, 에러, 서버 상태 변경을 MSW로 제어해 검증하는 방법을 다룬다.

## 핵심 내용
- 문제의 출발점은 검색 페이지처럼 입력 컴포넌트, API 호출, 결과 리스트가 함께 동작하는 화면이다. 서버 API가 없거나 원하는 데이터를 주지 않으면 통합 테스트가 깨지고, 핵심 기능 검증이 뒤로 밀린다.
- MSW는 API 요청을 가로채 사전에 설정한 mock 데이터를 반환한다. API 스펙만 합의되어 있으면 실제 서버 없이도 프론트엔드 통합 흐름을 검증할 수 있다.
- 기본 구조는 `src/mocks/handlers.ts`, `src/mocks/server.ts`, `setupTests.ts` 또는 `jest.setup.js`로 잡는다.
- handler는 연관 API 단위로 분리한다. 예시는 `mocks/api/searchResult.ts`, `mocks/api/data/searchResultData.ts`처럼 API handler와 응답 data를 나눈다.
- `server.use(rest.get(...))`로 테스트 실행 중 특정 API 응답을 덮어쓸 수 있다.
- 주요 검증 대상은 데이터 조회, 응답 데이터에 따른 조건부 렌더링, 빈 데이터 같은 엣지 케이스, 404/403 같은 네트워크 오류 처리다.
- 에러 처리는 화면 fallback뿐 아니라 axios interceptor, ErrorBoundary, 공통 error handler 호출 검증으로 확장될 수 있다.
- Storybook과 개발 서버에서도 MSW를 활용할 수 있다.

## 위키 반영
- [[MSW 통합 테스트]] — MSW 요청 가로채기, 성공·빈·오류, 서버 상태 변경
- [[통합 테스트]] — 컴포넌트와 API 함께 테스트

## 출처
- 카카오엔터테인먼트 테크블로그, MSW를 활용하는 Front-End 통합테스트: https://tech.kakaoent.com/front-end/2022/220825-msw-integration-testing/
- 로컬 원문: `10_raw/testing-sources/카카오엔터 MSW 통합 테스트 원문.txt`
